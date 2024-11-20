pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Tests') {
            parallel {
                stage('Unit tests'){
                    agent {
                        docker{
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }

                    steps {
                        sh '''
                            #test -f build/index.html
                            npm test
                        '''
                    }
                     post {
                        always {
                            junit 'test-results/junit.xml'
                        }
                    }
                }

                stage('E2E') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                            reuseNode true
                        }
                    }

                    steps{
                        sh '''
                            npm install serve
                            node_modules/.bin/serve -s build & sleep 10
                            npx playwright test --reporter=html
                        '''
                    }                   
                }
            }
        }               
    

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    apt-get update && apt-get install -y \
                    build-essential \
                    libcairo2-dev \
                    libjpeg-dev \
                    libpango1.0-dev \
                    libgif-dev \
                    librsvg2-dev \
                    libvips-dev

                    npm install netlify-cli --legacy-peer-deps
                    netlify --version 
                '''
            }
        }
    }
}
