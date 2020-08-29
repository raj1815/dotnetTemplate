"Dot.net Sample code"
def sqScannerMsBuildHome = "/home/jenkins/sonar-scanner-msbuild-4.8.0.12008"
pipeline {
    agent any 
    
    options {
        disableConcurrentBuilds()
    }
    
      environment {
        Nuget_Proxy = "https://api.nuget.org/v3/index.json"
        SonarQube_Project_Key = "NAGP.Net.Project"
      
    }
    
    stages {        
        stage('nudget restore') {
            steps {    
                bat "dotnet restore -s ${Nuget_Proxy}"
            }
        }
        
        
        stage("SonarQube Initialise") {
            steps {
                                script {
                    withSonarQubeEnv('SonarQube') {
                 bat "dotnet sonarscanner begin /k:${SonarQube_Project_Key}"          }
                }
                
            }
        }
        
        stage('Unit Testing') {
            steps {
                 echo 'testing'     
            }
        }
    
          stage('code build') {
            steps {    
                bat "dotnet build"  
            }
        }
    }
}
