
pipeline {
    agent any 
    
    options {
        disableConcurrentBuilds()
    }
    
      environment {
        Nuget_Proxy = "https://api.nuget.org/v3/index.json"
    }
    
    stages {        
        stage('nudget restore') {
            steps {    
                bat "dotnet restore -s ${Nuget_Proxy}"
                bat "dotnet build"  
            }
        }
        
        
        stage('sonar start ') {
            steps {    
                bat "dotnet build"  
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
