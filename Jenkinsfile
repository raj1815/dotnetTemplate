// -----------------------------------------------------------------------------------------
//
// Global variables
//
def timestamp = new Date().format("yyyy-MM-dd-HH-mm")
def sqScannerMsBuildHome = "/home/jenkins/sonar-scanner-msbuild-4.8.0.12008"

// -----------------------------------------------------------------------------------------
//
// The main pipeline section
//

def getVersion = {
    def contents = readFile('*.csproj')
    matcher = contents =~ '<AssemblyVersion>(.+)</AssemblyVersion>'
    version = matcher[0][1]
}

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
        stage('DotNet Build') {
            steps {                
                sh "dotnet restore -s ${Nuget_Proxy}"
                sh "dotnet build"                
            }
        }

        stage('Unit Testing') {
            steps {
                script {
                    try {
                        echo "=====> Running unit tests"
                        // Run all the unit tests for this project, outputting the results to a MS Test (.trx) file
                        //sh "dotnet test --logger 'trx;LogFileName=TestResults.trx' --no-build /p:CollectCoverage=true /p:CoverletOutputFormat=opencover /p:CoverletOutput=\"${WORKSPACE}/${env.Test_Output}/coverage.xml\" \"${env.Test_Project}\""                    
                    } catch (err) {
                        echo err.getMessage()
                        echo "Error detected, but we will continue."
                    }
                }
            }
        }

        stage ('SonarQube End') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        // Complete the build process and upload the results to the SonarQube server for processing
                        sh "dotnet ${sqScannerMsBuildHome}/SonarScanner.MSBuild.dll end /d:sonar.login=${SONAR_AUTH_TOKEN}"
                    }
                }
            }
        }      
        
        stage('DotNet Publish') {
            steps {
                dir("${env.Project_Folder}") {
                    sh "rm -rf out"
                    sh "dotnet publish -c Release -o out"                   
                }
            }
        }

    }
}
