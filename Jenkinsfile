pipeline{
    agent any
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo "Now Archiving..."
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to Staging'){
            steps{
                build job: 'deploy-to-stage'
            }
        }
        stage('Deploy to Production'){
            steps{
                timeout(times:5, unit:'DAYS'){
                    input message: 'Approve production deployment?'
                }
                build job: 'deploy-to-prod'
                success{
                    echo 'Code deployed to production.'
                }
                failure{
                    echo 'Deployment failed.'
                }
            }
        }
    }
}