def sqScannerMsBuildHome = tool 'Scanner for MSBuild 4.6'
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
                script{
                    withSonarQubeEnv('My SonarQube Server') {
      bat "${sqScannerMsBuildHome}\\SonarQube.Scanner.MSBuild.exe begin /k:myKey"

    }
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
