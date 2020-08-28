
pipeline {
    agent any 
    
    options {
        disableConcurrentBuilds()
    }
    
    stages {        
        stage('DotNet Build') {
            steps {                
               dotnet restore -s https://api.nuget.org/v3/index.json
                dotnet build             
            }
        }

        stage('Unit Testing') {
            steps {
                 echo 'testing'     
            }
        }
    
    }
}
