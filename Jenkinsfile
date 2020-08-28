
pipeline {
    agent any 
    
    options {
        disableConcurrentBuilds()
    }
    
      environment {
        Nuget_Proxy = "https://api.nuget.org/v3/index.json"
        SonarQube_Project_Key = "Lending.Repayment.Strategy.System.API"
        SonarQube_Project_Name = "Lending.Repayment.Strategy.System.API"
        SonarQube_Project_Exclusions = "**/*.json"
        SonarQube_Version = "1.0.0"
        Test_Project = ""
        Test_Output = ""
        Project_Solution = "WebApplication4.sln"
        Project_Folder = "WebApplication4"
        Publish_Path = "WebApplication4/out"
        UCD_Component_Name = "Lending.Repayment.Strategy.System.API"
    }
    
    stages {        
        stage('nudget restore') {
            steps {    
                bat "dotnet restore -s ${Nuget_Proxy}"
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
