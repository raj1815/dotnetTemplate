
pipeline
{
    agent any

    options {
        disableConcurrentBuilds()
    }

    environment {
        Nuget_Proxy = 'https://api.nuget.org/v3/index.json'
        Scan_path = 'C:/Users/raj1815/.dotnet/tools/dotnet-sonarscanner'
        SonarQube_Project_Key = 'sqs:NAGP-Assignment'
        SonarQube_Project_Name = 'sqs:NAGP-Assignment'
        SonarQube_Project_Exclusions = '**/*.json'
        SonarQube_Version = '1.0.0'
        scannerHome = tool name: 'sonar_scanner_dotnet', type: 'hudson.plugins.sonar.MsBuildSQRunnerInstallation'
    }

    stages {
        stage('nuget restore') {
            steps {
                bat'dotnet clean'
                bat 'dotnet restore'
            }
        }

        stage('Start sonarcube analysis') {
            steps {
                 echo 'Start sonarcube analysis'
                // withSonarQubeEnv('Test_Sonar') {
                //         bat "dotnet ${scannerHome}/SonarScanner.MsBuild.dll begin /k:\"sqs:NAGP-Assignment\"  /n:\"sqs:NAGP-Assignment\" /v:\"1.0.0\"  "
                // }
            }
        }

        stage('code build') {
            steps {
                bat 'dotnet publish -c release -o app --no-restore'
            }
        }

        stage('Stop sonarcube analysis') {
            steps {
                echo 'Start sonarcube analysis'
                // withSonarQubeEnv('Test_Sonar') {
                //     bat "dotnet ${scannerHome}/SonarScanner.MsBuild.dll end"
                // }
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t  i_raj1815_master:${BUILD_NUMBER} ."
        
            }
            
        }

            stage('Run Tests') {
            parallel {
                stage('Pre container check') {
                    steps {
                        script {
                            containerID = powershell(returnStdout: true, script:'docker ps --filter name=c_raj1815_master --format "{{.ID}}"')
                            if (containerID) {
                                bat "docker stop ${containerID}"
                                bat "docker rm -f ${containerID}"
                            }
                        }
                    }
                }
                stage('Push to Dtr') {
                    steps {
                        bat "docker tag i_raj1815_master rajk078/i_raj1815_master:${BUILD_NUMBER}"
                        // bat "docker push rajk078/i_raj1815_master:${BUILD_NUMBER}"
                    }
                }
            }
            }

        stage('Docker deployment') {
                steps {
                bat "docker run -d -p 8081:80 --name c_raj1815_master i_raj1815_master:${BUILD_NUMBER}"
                }
        }

   
        stage('deploy to kubernetes cluster') {

                steps {

withKubeConfig(caCertificate: '''apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://kubernetes.docker.internal:6443
  name: docker-desktop
contexts:
- context:
    cluster: docker-desktop
    user: docker-desktop
  name: docker-desktop
- context:
    cluster: docker-desktop
    user: docker-desktop
  name: docker-for-desktop
current-context: docker-desktop
kind: Config
preferences: {}
users:
- name: docker-desktop
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED''', clusterName: 'docker-desktop', contextName: 'docker-desktop', credentialsId: '5ec3ed52-8513-4070-af4b-d2b63263e783', namespace: '', serverUrl: 'https://kubernetes.docker.internal:6443') {
      bat 'kubectl get pods'
}


 }                   
                
            }

    }
}
