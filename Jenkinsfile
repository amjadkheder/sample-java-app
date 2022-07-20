pipeline {
    agent any
    environment{   
        AWS_S3 = "pipeline-eb"
        ARTIFACT_NAME = "hello-world.war"

    }


    stages {
        stage('Validate') {
            steps {
                sh "mvn validate"
                sh "mvn clean"
            }
        }

        stage('Build') {
            steps {
                sh "mvn compile"
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
                
            }
       
            post {
                always{
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                sh "mvn package"
                
            }
            post{
                success{
                    archiveArtifacts artifacts: '**/target/**.war', followSymlinks: false
                }
            }
        }
    
        stage('publish artfacts to s3') {
            steps {
                sh "aws configure set region us-east-1" 
                sh "aws s3 cp ./target/**.war s3://$AWS_S3/$ARTIFACT_NAME"
            }
        }

        stage('deploy') {
            steps {
            echo "last stage"
            
            }
        }
    }
}

