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
                    npm ci --unsafe-perm
                    npm run build --unsafe-perm
                    ls -la
                '''
            }
        }

        stage('Test') {
            steps {
                sh 'test -f build/index.html'
            }
        }
    }
}
