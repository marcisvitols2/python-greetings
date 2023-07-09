pipeline {
    agent any
    triggers{ pollSCM('*/1 * * * *') }
    

    stages {
        stage('Build docker image') {
            steps {
                script{
                    build()
                }
            }
        }
        stage('Deploy to dev environment') {
            steps {
                script{
                    deploy("dev")
                }
            }
        }
        stage('Tests on dev environment') {
            steps {
                script{
                    test("dev")
                }
            }
        }
        stage('Deploy to stg environment') {
            steps {
                script{
                    deploy("stg")
                }
            }
        }
        stage('Tests on stg environment') {
            steps {
                script{
                    test("staging")
                }
            }
        }
        stage('Deploy to prod environment') {
            steps {
                script{
                    deploy("prod")
                }
            }
        }
        stage('Tests on prod environment') {
            steps {
                script{
                    test("prod")
                }
            }
        }
    }
}



def build(){
    echo "Building and pushing docker image."
    git branch: 'main', url: 'https://github.com/marcisvitols2/python-greetings'

    sh "docker build --no-cache -t marcisvitols/python-greetings-app:latest . "
    sh "docker push marcisvitols/python-greetings-app:latest"

}

def deploy(String environment){
    echo "Deployment python microserivce to ${environment} has started.."
   
    sh "docker pull marcisvitols/python-greetings-app:latest"
    sh "docker-compose stop greetings-app-${environment}"
    sh "docker-compose rm greetings-app-${environment}"
    sh "docker-compose up -d greetings-app-${environment}"

}

def test(String environment){
    echo "Testing on ${environment} has started.."
    
    sh "docker pull marcisvitols/api-tests:latest"
    sh "docker run --network=host --rm marcisvitols/api-tests:latest run greetings greetings_${environment}"



}