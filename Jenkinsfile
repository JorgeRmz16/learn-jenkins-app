pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh'''
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
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh'''
                    echo "Test stage"
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('E2E'){
            agent {
                docker{
                    image 'mcr.microsoft.com/playwright:v1.39.0-noble'
                    reuseNode true
                    // run as administrator user (not recomend to use)
                    // args '-u root:root' 
                }
            }
            steps{
                sh'''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                '''
                // use & at the end to run the next command without wait the end of a step
            }
        }
    }
    post{
        always{
            junit 'jest-results/junit.xml'
        }
    }
}
