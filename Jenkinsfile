pipeline{
    agent {
        node {
            label 'agent-1'
        }
    }
    environment {
        packageVersion=''
        nexusURL='172.31.1.197:8081' //prot No: 8081
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
                    packageVersion = packageJson.version
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
        stage('publish Artifact'){ //Install nexusArtifactUploader plugin in jenkins
            steps{
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusURL}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth', //we need to create creds in jenkins
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
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