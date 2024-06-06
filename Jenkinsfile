pipeline {
  agent any
    stages {
      stage('docker build') {
        steps {
          sh 'docker build -t mena5800/node-farm .'
        }
      }

      stage('aws credientials') {
        steps {
            withCredentials([
                    usernamePassword(credentialsId: 'aws_credentials', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                      sh '''
                        aws configure set aws_access_key_id ${AWS_ACCESS_KEY_ID}
                        aws configure set aws_secret_access_key ${AWS_SECRET_ACCESS_KEY}
                        aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 975050377735.dkr.ecr.eu-west-1.amazonaws.com
                        '''
                }
        }
      }

      stage('docker tag and push') {
        steps{
            sh "docker tag mena5800/node-farm 975050377735.dkr.ecr.eu-west-1.amazonaws.com/lambda-test:latest"
            sh "docker push 975050377735.dkr.ecr.eu-west-1.amazonaws.com/lambda-test:latest"
        }
      }
    }
  }