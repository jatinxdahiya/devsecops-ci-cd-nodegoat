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
        
        stage('Deploy to AWS EC2') {
            steps {
                sh """
                ssh ubuntu@3.110.207.223 << 'EOF'
                  if [ ! -d NodeGoat ]; then
                    git clone https://github.com/jatinxdahiya/NodeGoat.git
                  fi
                  cd NodeGoat
                  docker compose down || true
                  docker compose up -d --build
                EOF
                """
            }
        }

        stage('Build Docker Image'){
            steps{
                sh '''
                echo "Building Docker image..."
                docker build -t nodegoat-devops .
                '''
            }
        }

        stage('Deploy Application'){
            steps{
                sh '''
                echo "Stoppinf existing containers (if any)..."
                docker compose down || true

                echo "Starting application..."
                docker compose up -d
                '''
            }
        }

        stage('Verify Deployment'){
            steps{
                sh '''
                echo "Running Containers:"
                docker ps

                echo "Checking application port..."
                sleep 5
                curl -f http://localhost:4000 || echo "App may still be starting"
                '''
            }
        }
    }

    post{
        success{
            echo '✅ Deployment successful'
        }
        failure{
            echo '❌ Deployment failed'
        }
    }
}
