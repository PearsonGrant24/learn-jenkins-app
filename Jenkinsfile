pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'noe:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    nom run build
                    ls -la
                '''
            }
        }
    }
}
