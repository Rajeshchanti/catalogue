pipeline{
    agent {
        node {
            label 'agent-1'
        }
    }
    environment {
        packageVersion=''
    }
    options{
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    stages{
        stage('Get the Version') {
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.Version
                    echo "application Version: $packageVersion"
                }
            }
        }
        stage('Install Dependencies') {
            steps{
                sh """
                    npm install
                """
            }
        }
        stage('Build') {
            steps{
                sh """
                    ls -la
                    zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -ltr
                """
            }
        }
    }
    post {
        always {
            echo 'job is done'
            deleteDir()
        }
        failure {
            echo 'Build is Failed'
        }
        success {
            echo 'pipeline is successfully build'
        }
    }
}