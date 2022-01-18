pipeline {
  agent any
  
  parameters{
    string(name:'tomcat_dev',
    defaultValue:'/opt/tomcat/webapps',
    description:'Staging Server : 8080',
    string(name:'tomcat_prod',
    defaultValue:'/opt/tomcat2/webapps',
    description:'Production Server : 8080'
  }
           
           triggers {pollSCM('* * * * *')}//Polling Source Control
           
           stages {
             stage('build'){
               steps{bat 'mvn clean package'}
               post{
                 success{
                 echo "Now Archiving..."
                   archiveArtifacts artifacts:'**/target/*.war'
                 }
               }
             }
             stage('Deployments'){
               parallel{
                  stage('Deploy to Staging'){
                    steps {
                      bat "cp **/target/*.war $tomcat_dev$"
                           }
                         }
                 
                 stage('Deploy to production'){
                    steps {
                      bat "cp **/target/*.war $tomcat_prod$"
                           }
                         }
                       }
             }
           }
}
