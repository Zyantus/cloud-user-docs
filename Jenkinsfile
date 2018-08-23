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
		   ls -la /home/
		   sleep 60
		   ssh-keygen -f "/home/dummy/.ssh/known_hosts" -R $FLOATINGIP
		   ssh -i /home/dummy/Jenkins.pem ubuntu@$(echo $FLOATINGIP) ifconfig
		   echo "Hello"
		   exit
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
