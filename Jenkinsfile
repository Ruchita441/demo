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
    stage('Upload war file to nexus'){
      steps{
        script{
          def readPomVersion = readMavenPom file: 'pom.xml'
          def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" : "demoapp"
          nexusArtifactUploader artifacts: 
           [
              [
                artifactId: 'springboot', 
                classifier: '', file: 'target/Uber.jar', 
                type: 'jar'
                ]
            ], 
           credentialsId: 'nexus-auth2', 
           groupId: 'com.example', 
           nexusUrl: '13.233.229.55:8081', 
           nexusVersion: 'nexus3', 
           protocol: 'http', 
           repository: nexusRepo, 
           version: "${readPomVersion.version}"
        }
      }
    }
  } 
}



   
