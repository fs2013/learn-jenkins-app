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
                    # Fix permissions for .npm cache
                    mkdir -p ~/.npm && sudo chown -R $(id -u):$(id -g) ~/.npm

                    # Display directory contents
                    ls -la

                    # Verify Node.js and npm versions
                    node --version
                    npm --version

                    # Install dependencies and build
                    npm ci --unsafe-perm
                    npm run build --unsafe-perm

                    # Display build output
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    # Fix permissions for .npm cache
                    mkdir -p ~/.npm && sudo chown -R $(id -u):$(id -g) ~/.npm

                    # Verify test artifacts exist
                    test -f build/index.html

                    # Run tests
                    npm test --unsafe-perm
                '''
            }
        }
    }
}
