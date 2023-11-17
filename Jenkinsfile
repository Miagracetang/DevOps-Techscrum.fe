pipeline {
    agent any

    parameters {
        string(name: 'AWS_CREDENTIAL_ID', defaultValue: 'miagracetang-at-145367278427', description: 'The ID of the AWS credentials to use')
        string(name: 'S3_BUCKET', defaultValue: '', description: 'The name of the S3 bucket to deploy to')
        string(name: 'GIT_BRANCH', defaultValue: 'main', description: 'The Git branch to build and deploy')
    }


    stages {
        stage('test AWS credentials') {
            steps {
                withAWS(credentials: '8e63c1fe-a242-4779-bddc-17569e9888c8', region: 'ap-southeast-2') {
                    sh 'echo "hello Jenkins">hello.txt'
                    s3Upload acl: 'Private', bucket: 'devopslee', file: 'hello.txt'
                    s3Download bucket: 'devopslee', file: 'downloadedHello.txt', path: 'hello.txt'
                    sh 'cat downloadedHello.txt'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
            // slackSend (color: '#00FF00', message: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        failure {
            echo 'Deployment failed!'
            // slackSend (color: '#ff3700', message: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
    }