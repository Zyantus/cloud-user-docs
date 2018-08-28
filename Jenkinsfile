pipeline {
    agent any

    environment {
	DISABLE_AUTH = 'true'
	DB_ENGINE = 'sqlite'
    }

    stages {
        stage('TESTING') {
            steps {
                echo 'Bau Virt.Masch.:'
                sh '''#!/bin/bash
		   git rev-parse --abbrev-ref HEAD > GIT_BRANCH
		   echo $GIT_BRANCH | awk -F '/' '{ print $NF }' > /home/dummy/GITBRANCH
		   echo $GIT_BRANCH
		   cat /home/dummy/GITBRANCH
		   GITBRANCH=$(cat /home/dummy/GITBRANCH)
		   echo $GITBRANCH 
		   exit 1 
		   source /home/dummy/CloudComputing-openrc.sh
                   openstack server create --key-name Jenkins --image 'Ubuntu 16.04 LTS (2018-08-16)' --flavor de.NBI.small --network 'internal' $GITBRANCH 
		   FLOATINGIP=$(openstack floating ip create external| grep floating_ip| cut -d'|' -f3)
		   openstack server add floating ip $GIT_BRANCH $FLOATINGIP
		   echo $FLOATINGIP
		   sleep 150
		   echo $FLOATINGIP
		   rm /home/dummy/.ssh/known_hosts
		   ssh -oStrictHostKeyChecking=no -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) 'sudo apt install -y ansible unzip python-apt git; git pull -b $GITBRANCH https://github.com/Zyantus/cloud-user-docs/;'
		   [ $(ssh -oStrictHostKeyChecking=no -n -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) 'cd cloud-user-docs-master/AnsibleRoles; ansible-playbook playbook.yml | grep 'failed=' > FAIL; cat FAIL | rev | cut -d"=" -f1 | rev ') -gt 0 ] && echo "FEHLER!!!" && openstack floating ip delete $FLOATINGIP && openstack server delete $GITBRANCH && exit 1
		   echo "Platz fuer Tests:"
		   openstack floating ip delete $FLOATINGIP
		   openstack server delete $GITBRANCH
		'''
            }
        }
    }
}
