pipeline {
  agent any
  
  parameters{
    string(name:'tomcat_dev',
    defaultValue:'/opt/tomcat/apache-tomcat-9.0.54/webapps',
    description:'Staging Server : 8080',)
    string(name:'tomcat_prod',
    defaultValue:'/opt/tomcat2/apache-tomcat-9.0.54/webapps',
    description:'Production Server : 8090')
  }
           
  triggers {pollSCM('* * * * *')}//Polling Source Control
           
           stages {
             stage('build'){
               steps{sh 'mvn clean package'}
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
                      sh "cp **/target/*.war $tomcat_dev"
                           }
                         }
                 
                 stage('Deploy to production'){
                    steps {
                      sh "cp **/target/*.war $tomcat_prod"
                           }
                         }
                       }
             }
           }
}
