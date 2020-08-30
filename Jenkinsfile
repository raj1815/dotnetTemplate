  
pipeline {
    agent any
   
    options {
        disableConcurrentBuilds()
    }
   
      environment {
        Nuget_Proxy = "https://api.nuget.org/v3/index.json"
        Scan_path = "C:/Users/raj1815/.dotnet/tools/dotnet-sonarscanner"
    }
   
    stages {       
        stage('nuget restore') {
            steps {   
                bat"dotnet clean"
                bat "dotnet restore"
             
            }
        }
       
      
        stage('Unit Testing') {
            steps {
                 echo 'testing'    
            }
        }
   
        stage('Sonar analysis begin') {
                steps {
                   bat "${Scan_path} begin  /k:\"sqs:NAGP-Assignment\""
                }
            }
          stage('code build') {
            steps {   
                bat "dotnet build"
            }
        }


             stage('Sonar analysis end') {
            steps {
                 bat "${Scan_path} end"  
            }
        }
    }
}
