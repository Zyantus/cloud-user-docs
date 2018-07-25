pipeline {
    agent any

    stages {
        stage('VM Creation') {
            steps {
                echo 'Liste der Flavor:'
                openstack flavor list
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
