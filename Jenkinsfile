  
pipeline {
    agent any
   
    options {
        disableConcurrentBuilds()
    }
   
      environment {
        Nuget_Proxy = "https://api.nuget.org/v3/index.json"
        Scan_path = "C:/Users/raj1815/.dotnet/tools/dotnet-sonarscanner"
        SonarQube_Project_Key = "sqs:NAGP-Assignment"
        SonarQube_Project_Name = "sqs:NAGP-Assignment"
        SonarQube_Project_Exclusions = "**/*.json"
        SonarQube_Version = "1.0.0"
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
                    bat "${Scan_path} begin /k: \"sqs:NAGP-Assignment\"" \
                            /v:'${env.SonarQube_Version}' \
                            /d:sonar.buildbreaker.skip=\"true\" \
                            /d:sonar.exclusions='${env.SonarQube_Project_Exclusions}'"

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
