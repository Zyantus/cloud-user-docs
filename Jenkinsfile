pipeline {
    agent any

    stages {
        stage('VM Creation') {
            steps {
                echo 'Bau Virt.Masch.:'
                sh '''#!/bin/bash
                   openstack server create --key-name Jenkins --image 'Ubuntu 16.04 LTS (2018-08-16)' --flavor de.NBI.small --network 'internal' Jenkinstest
		   FLOATINGIP=$(openstack floating ip create external| grep floating_ip| cut -d'|' -f3)
		   openstack server add floating ip Jenkinstest $FLOATINGIP
		   echo $FLOATINGIP
		   sleep 300
		   echo $FLOATINGIP
		   rm /home/dummy/.ssh/known_hosts
		   ssh -oStrictHostKeyChecking=no -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) 'sudo apt install -y ansible unzip python-apt; wget https://github.com/Zyantus/cloud-user-docs/archive/master.zip; unzip master.zip; cd cloud-user-docs-master/AnsibleRoles; ansible-playbook playbook.yml'
		   ssh -oStrictHostKeyChecking=no -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) 'false'
		   openstack floating ip delete $FLOATINGIP
		'''
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
		sh '''#!/bin/bash
		'''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
		sh '''#!/bin/bash
                   openstack server delete Jenkinstest
                '''
            }
        }
    }
}
