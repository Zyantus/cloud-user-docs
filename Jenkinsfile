pipeline {
    agent any

    stages {
        stage('VM Creation') {
            steps {
                echo 'Liste der Flavor:'
                sh '''#!/bin/bash
                sudo apt install openstack
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
