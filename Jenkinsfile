
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

        stage('Sonar analysis begin') {
            steps {
                bat "${Scan_path} begin /k:\"sqs:NAGP-Assignment\"  /n:\"sqs:NAGP-Assignment\" /v:\"1.0.0\"  "
                 }
        }
        
        
        stage('code build') {
            steps {
                bat "dotnet publish -c release -o app --no-restore"
            }
        }

       stage('Sonar analysis end') {
            steps {
                echo "analysis end"
               //  bat "${Scan_path} end"  
            }
        }
        
      stage('Docker Build') {
      steps {
        bat 'docker build -t nagp-net-assignment:latest .'
      }
        }


            stage('Run Tests') {
            parallel {
                stage('Pre container check') {
                    steps {
                           script{
                containerID = powershell(returnStdout: true, script:'docker ps --filter name=c_rajkumar_master --format "{{.ID}}"')
                  if(containerID)
                    {
                        bat "docker stop ${containerID}"
                        bat "docker rm -f ${containerID}"
                     }
                  }
                    }
                }
                stage('Push to Dtr') {
                    steps {
                        echo "echo Test On Linux"
                    }
                }
            }
        }
        
        [6:28 PM] Ajay Srivastava
    

pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

      environment {
Nuget_Proxy = "https://api.nuget.org/v3/index.json"
Scan_path = "C:/Users/ajaysrivastava/.dotnet/tools/dotnet-sonarscanner"
    }

    stages {       
        stage('nuget restore') {
            steps {   
                bat"dotnet clean"
                bat "dotnet restore"

            }
        }


          stage('code build') {
            steps {   
                bat " dotnet build -c Release"
            }
        }
          stage('docker image build') {
            steps { 
                bat " docker build -t i_ajaysrivastava_master ."
            }

        }
    stage('docker config') {
        parallel {
          stage('docker container precheck') {
              agent any
              steps {
                  script{
                containerID = powershell(returnStdout: true, script:'docker ps --filter name=c_ajaysrivastava_master --format "{{.ID}}"')
if(containerID)
                    {
                        bat "docker stop ${containerID}"
                        bat "docker rm -f ${containerID}"
                     }
                  }
              }
          }

            stage('push docker image to dtr') {
                agent any
                steps {   
                            bat "docker tag i_ajaysrivastava_master dtr.nagarro.com:443/i_ajaysrivastava_master"
                            bat "docker push dtr.nagarro.com:443/i_ajaysrivastava_master"
                        }
            }
        }
    }
 
            stage('docker image run') {
                steps {   
                            bat "docker run -d -p 8081:80 --name c_rajkumar_master nagp-net-assignment"
                        }
            }
 

 
    }
}


        
        
    }
}
