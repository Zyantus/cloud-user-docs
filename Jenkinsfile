pipeline {
    agent any

    stages {
        stage('VM Creation') {
            steps {
                echo 'Liste der Flavor:'
                ls -la
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
