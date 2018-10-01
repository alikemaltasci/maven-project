pipeline {
    agent any

    parameters {
         string(name: 'pem_path', defaultValue: '/Users/alikemaltasci/Ali Kemal/Eğitimler/Udemy_Master_Jenkins/AWS/tomcat-demo.pem', description: 'pem file location')
         string(name: 'tomcat_stg', defaultValue: 'ec2-18-224-177-111.us-east-2.compute.amazonaws.com', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'ec2-18-222-227-126.us-east-2.compute.amazonaws.com', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i ${params.pem_path} **/target/*.war ec2-user@${params.tomcat_stg}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i ${params.pem_path} **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
