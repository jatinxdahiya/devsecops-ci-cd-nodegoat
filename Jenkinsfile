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
                withSonarQubeEnv('Sonarqube') {
                      script {
                         def scannerHome = tool 'SonarScanner'
                         sh """
                         ${scannerHome}/bin/sonar-scanner \
                         -Dsonar.projectKey=nodegoat-devsecops \
                         -Dsonar.sources=.
                         """
                      }
                }
            }
         }


         stage('Quality Gate'){
            steps{
                timeout(time: 2, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
         }

         stage('SCA - Dependency Check'){
            steps{
                dependencyCheck additionalArguments: '''
                    --scan .
                    --format HTML
                    --out dependency-check-report
                ''',
                odcInstallation: 'DependencyCheck'

                dependencyCheckPublisher pattern: '**/dependency-check-report/dependency-check-report.html'
            }
         }

         stage('DAST - OWASP ZAP'){
            steps{
                sh '''
                docker run --rm \
                    ghcr.io/zaproxy/zaproxy:stable \
                    zap-baseline.py \
                    -t http://192.168.0.108:4000 \
                    -r zap-report.html || true
                '''

                archiveArtifacts artifacts: 'zap-report.html', fingerprint: true
            }
         }
    }
    post{
        success{
            echo '✅ DevSecOps completed successfully'
        }
        failure{
            echo '❌ DevSecOps failed'
        }
    }
}

