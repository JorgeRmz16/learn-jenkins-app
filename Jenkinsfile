pipeline {
    agent {
        docker{
            image 'node:18-alpine'
            reuseeNode true
        }
    }

    stages {
        stage('Hello') {
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
    }
}
