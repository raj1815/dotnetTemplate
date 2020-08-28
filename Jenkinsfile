
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
        
         stage("Start sonarqube analysis") {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        // Initialise the MS Build step with the SonarQube hooks to collect information for upload and analysis later. 
                        // Replace /k:<key> with registered key from https://sonarqube.nednet.co.za
                        bat "dotnet ${sqScannerMsBuildHome}/SonarScanner.MSBuild.dll begin /k:'${env.SonarQube_Project_Key}' \
                            /d:sonar.host.url='${SONAR_HOST_URL}' \
                            /d:sonar.login='${SONAR_AUTH_TOKEN}' \
                            /v:'${env.SonarQube_Version}' \
                            /d:sonar.branch.name='${BRANCH_NAME}' \
                            /d:sonar.buildbreaker.skip=\"true\" \
                            /d:sonar.exclusions='${env.SonarQube_Project_Exclusions}'"

                            //d:sonar.cs.opencover.reportsPaths='${WORKSPACE}/${env.Test_Output}/coverage.xml' \
                            //d:sonar.cs.vstest.reportsPaths='${WORKSPACE}/${env.Test_Output}/TestResults.trx'"
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
