pipeline{
    agent{label 'ubuntu-node'}

    options{
        timestamps()
    }

    stages{

        stage('Checkout'){
            steps{
                checkout scm
            }
        }
        
    
        stage('SAST - SonarQube') {
             steps {
                 script {
                     def scannerHome = tool 'SonarQubeScanner'
                     

                     withSonarQubeEnv('Sonarqube') {
                      sh """
                          ${scannerHome}/bin/sonar-scanner \
                          -Dsonar.projectKey=nodegoat-devsecops \
                          -Dsonar.sources=. \
                          -Dsonar.language=js
                      """
                     }
                 }
             }
         }
    }
    post{
        success{
            echo '✅ SAST completed successfully'
        }
        failure{
            echo '❌ SAST failed'
        }
    }
}
