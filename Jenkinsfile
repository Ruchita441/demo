pipeline {
  
  agent any
  
  stages{
    
    stage('Git Checkout'){
      
      steps{
        git branch: 'main' , url: 'https://github.com/Ruchita441/demo.git' 
      }
    }
    stage('Unit Testing'){
      
      steps{
        sh 'mvn test'
      }
    }
    stage('Integration Testing'){
      
      steps{
        sh 'mvn verify -DskipUnitTests'
      }
    }
     stage('Maven Building'){
      
      steps{
        sh 'mvn clean install'
      }
    }
    stage('Static code analysis'){
            
      steps{       
        script{
          withSonarQubeEnv(credentialsId: 'sonar') {
           sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
    stage('Quality Gate Status'){
      steps{           
        script{ 
          waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                 }
               }
            }
        }
        
}
