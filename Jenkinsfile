pipeline {
    agent any

    stages {
        stage('VM Creation') {
            steps {
                echo 'Bau Virt.Masch.:'
                sh '''#!/bin/bash
                   openstack server create --key-name Jenkins --image 'Ubuntu 16.04 LTS (2018-08-16)' --flavor de.NBI.small --network 'internal-cebitec' Jenkinstest
		'''
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
		ssh '''#!/bin/bash
		   TESTIPTEST=$(openstack floating ip list| grep None| grep 172.21| cut -d'|' -f3)
		   echo TESTIPTEST
		'''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
		ssh '''#!/bin/bash
                   openstack server delete Jenkinstest
                '''
            }
        }
    }
}
