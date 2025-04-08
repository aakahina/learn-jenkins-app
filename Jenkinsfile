pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'fc1c29e9-1c2b-4521-9399-67559e75287c'
    }

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

        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                 test -f build/index.html
                 npm test
                '''
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
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploy to fc1c29e9-1c2b-4521-9399-67559e75287c"
                 '''
            }
        }
        
    }
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
