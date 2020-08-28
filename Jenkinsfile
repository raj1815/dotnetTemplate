
pipeline {
    agent any 
    
    options {
        disableConcurrentBuilds()
    }
    
    stages {        
        stage('DotNet Build') {
            steps {                
               echo 'build'             
            }
        }

        stage('Unit Testing') {
            steps {
                 echo 'testing'     
            }
        }
    
    }
}
