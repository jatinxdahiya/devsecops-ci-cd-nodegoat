pipeline{
    agent { label 'ubuntu-node' }

    stages{

        stage('Checkout'){
            steps{
                dir('.'){
                    git branch: 'master', url: 'https://github.com/jatinxdahiya/NodeGoat.git'
                }
            }
        }

        stage('Build Docker Image'){
            steps{
                dir('.'){
                    sh 'docker build -t nodegoat-devops .'
                }
            }
        }

        stage('Deploy Application'){
            steps{
                dir('.'){
                    sh '''
                    docker compose down || true
                    docker compose up -d
                    '''
                }
            }
        }
    }
}
