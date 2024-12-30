pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:16-alpine'
                    reuseNode true
                }
            }
            environment {
                NPM_CONFIG_CACHE = '/var/jenkins/workspace/learn-jenkins-app/npm-cache' // Custom npm cache directory
                NODE_OPTIONS = '--openssl-legacy-provider' // Use legacy OpenSSL provider
            }
            steps {
                sh '''
                    # Create and fix permissions for npm cache directory
                    mkdir -p $NPM_CONFIG_CACHE && chown -R $(id -u):$(id -g) $NPM_CONFIG_CACHE

                    # Verify Node.js and npm versions
                    node --version
                    npm --version

                    # Install dependencies and build
                    npm ci --unsafe-perm
                    npm run build --unsafe-perm

                    # Display build output
                    ls -la build/
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:16-alpine'
                    reuseNode true
                }
            }
            environment {
                NPM_CONFIG_CACHE = '/var/jenkins/workspace/learn-jenkins-app/npm-cache' // Custom npm cache directory
            }
            steps {
                sh '''
                    # Create and fix permissions for npm cache directory
                    mkdir -p $NPM_CONFIG_CACHE && chown -R $(id -u):$(id -g) $NPM_CONFIG_CACHE

                    # Verify test artifacts exist
                    test -f build/index.html

                    # Run tests
                    npm test --unsafe-perm
                '''
            }
        }
    }
}
