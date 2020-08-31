
pipeline
{
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
                bat "${Scan_path} begin /k:\"sqs:NAGP-Assignment\"  /n:\"sqs:NAGP-Assignment\" /v:\"1.0.0\"  "

                 }
        }
        stage('code build') {
            steps {
                bat "dotnet publish -c release -o /app --no-restore"
            }
        }

      stage('Docker Build') {
      agent any
      steps {
        bat 'docker build -t nagp-net-assignment:latest .'
      }
        }

        stage('Sonar analysis end') {
            steps {
                echo "analysis end"
               //  bat "${Scan_path} end"  
            }
        }


    }
}
