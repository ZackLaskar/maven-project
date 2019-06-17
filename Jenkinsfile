pipeline {
    agent any
    
    tools {
        maven 'LocalMaven'
    }
    
    stages {
      stage('Build'){
        steps{
          sh 'mvn clean package'
        }
        post {
          success{
            echo "Now archiving..."
            archiveArtifacts artifacts: '**/target/*.war'
          }
        }
      }
        stage("Deploy to Staging"){
            steps{
                build job:"Deploy-to-staging"
            }
        }
         stage("Deploy to Production"){
             steps{
                 timeout(time:5, unit:'DAYS'){
                    input message: 'Approve production deployment?'
                 }
                build job: 'deploy-to-prod'
             }
             post {
                 success {
                    echo 'code deploy to production'
                 }
                 failure {
                    echo 'Deployment failed'
                 }
             }
     }
  }
}
