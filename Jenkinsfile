pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '/Users/dperdue/jenkins_project/apache-tomcat-8.5.28-production/webapps', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '/Users/dperdue/jenkins_project/apache-tomcat-8.5.28-staging/webapps', description: 'Production Server')
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
                        sh "cp **/target/*.war ${params.tomcat_dev}"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp  **/target/*.war ${params.tomcat_prod}"
                    }
                }
            }
        }
    }
}
