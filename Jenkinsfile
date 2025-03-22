pipeline {
     agent { label 'Jenkins-Agent' }
    tools  {
        jdk 'Java17'
        maven 'Maven3'
    }
  
    stages {
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }
      
        stage ("Checkout from SCM"){
                steps {
                git branch: 'main', credentialsId: 'github', 'https://github.com/Madhiravijay55/registration-app'
                }
        }

       stage ("Build Application"){
          steps {
               sh "mvn clean package"
          }

        }
              
      }
              
              }

          
                  
       
