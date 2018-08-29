pipeline {
    agent any

    environment {
	DISABLE_AUTH = 'true'
	DB_ENGINE = 'sqlite'
    }

    stages {
        stage('INIT VM') {
            steps {
                echo 'Bau Virt.Masch.:'
                sh '''#!/bin/bash
		   git rev-parse --abbrev-ref HEAD > GIT_BRANCH
		   echo $GIT_BRANCH | awk -F '/' '{ print $NF }' > /home/dummy/GITBRANCH
		   GITBRANCH=$(cat /home/dummy/GITBRANCH)
		   REPOSI=$(echo "https://github.com/Zyantus/cloud-user-docs/archive/$GITBRANCH.zip")
		   source /home/dummy/CloudComputing-openrc.sh
                   openstack server create --key-name Jenkins --image 'Ubuntu 16.04 LTS (2018-08-16)' --flavor de.NBI.small --network 'internal' $GITBRANCH 
		   FLOATINGIP=$(openstack floating ip create external| grep floating_ip| cut -d'|' -f3)
		   openstack server add floating ip $GITBRANCH $FLOATINGIP
		   sleep 150
		   rm /home/dummy/.ssh/known_hosts
		   ssh -oStrictHostKeyChecking=no -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) "sudo apt install -y ansible unzip python-apt; wget $REPOSI; unzip $GITBRANCH.zip; cd cloud-user-docs-$GITBRANCH/AnsibleRoles; ansible-playbook playbook.yml | grep 'failed' > /var/tmp/FAIL "
		   [ ! $(ssh -oStrictHostKeyChecking=no -n -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) 'grep 'failed=0' /var/tmp/FAIL' ) ] && echo "FEHLER!!!" &&  ssh -oStrictHostKeyChecking=no -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) 'cat /var/tmp/FAIL' && openstack floating ip delete $FLOATINGIP && openstack server delete $GITBRANCH && exit 1
		   ssh -oStrictHostKeyChecking=no -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) 'cat /var/tmp/FAIL'
		   echo "Platz fuer Tests:"
		   openstack floating ip delete $FLOATINGIP
		'''
            }
        }
        stage('TESTING') {
           steps {
                git rev-parse --abbrev-ref HEAD > GIT_BRANCH
                echo $GIT_BRANCH | awk -F '/' '{ print $NF }' > /home/dummy/GITBRANCH
                GITBRANCH=$(cat /home/dummy/GITBRANCH) 
                source /home/dummy/CloudComputing-openrc.sh
		FLOATINGIP=$(openstack floating ip create external| grep floating_ip| cut -d'|' -f3)
                openstack server add floating ip $GITBRANCH $FLOATINGIP
		sleep 150
		ssh -oStrictHostKeyChecking=no -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) "echo "INSERT TESTS" "
		openstack floating ip delete $FLOATINGIP
           }    
        }  

	stage('CLEAN') {
	   steps {
		git rev-parse --abbrev-ref HEAD > GIT_BRANCH
                echo $GIT_BRANCH | awk -F '/' '{ print $NF }' > /home/dummy/GITBRANCH
                GITBRANCH=$(cat /home/dummy/GITBRANCH)
		source /home/dummy/CloudComputing-openrc.sh
		openstack server delete $GITBRANCH
	   }
	}
    }
}
