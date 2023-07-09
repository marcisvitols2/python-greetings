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
                    test("stg")
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
    //git branch: 'main', url: 'https://github.com/mtararujs/sample-book-app.git'
    //sh "npm install"
    //sh "pm2 delete \"books-${environment}\""
    //sh "pm2 start -n \"books-${environment}\" index.js -- ${port}"

    echo "Deployment to ${environment} has started.."
    sh "docker pull marcisvitols/python-greetings-app:latest"
    sh "docker-compose stop greetings-app-${environment}"
    sh "docker-compose rm greetings-app-${environment}"
    sh "docker-compose up greetings-app-${environment}"

}

def test(String test_set, String environment){
    echo "Testing on ${environment} has started.."
    //git branch: 'main', poll: false, url: 'https://github.com/mtararujs/course-js-api-framework.git'
    //sh "ls"
    //sh "npm install"
    //sh "npm run ${test_set} ${test_set}_${environment}"

    sh "docker pull marcisvitols/api-tests:latest"
    sh "docker run --network=host --rm marcisvitols/api-tests:latest run greetings greetings_${environment}"



}