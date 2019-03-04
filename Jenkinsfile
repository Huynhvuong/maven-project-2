pipeline {
    agent any
 
    tools {
        maven 'localMaven'
    }
 
stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Stagging 2'){
              steps{
                build job: 'deploy-to-staging'
            }
        }
        stage ('Deploy to Production'){
            steps{
                timeout(time:1, unit:'MINUTES'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed success to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
          
    }
}
