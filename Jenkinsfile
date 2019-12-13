pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'JDK 1.8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                TOKEN1=5907dc5ded25e9cc675b64294d90aadc7553615c
                TOKEN2=66b4d4da233b7a5a599708f84cb096929344d715
                    curl "https://api.github.com/repos/diannyguarin/labs-backend/statuses/$GIT_COMMIT?access_token=${TOKEN1}${TOKEN2}" \
                      -H "Content-Type: application/json" \
                      -X POST \
                      -d "{\"state\": \"pending\",\"context\": \"continuous-integration/jenkins\", \"description\": \"Jenkins\", \"target_url\": \"$BUILD_URL\"}"
                '''
                sh '''
                    echo "PATH = ${PATH}"
                '''
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean install package spring-boot:repackage'
            }
        }
    }
    post{
        always{
            sh '''
            TOKEN1=5907dc5ded25e9cc675b64294d90aadc7553615c
            TOKEN2=66b4d4da233b7a5a599708f84cb096929344d715
                  curl "https://api.github.com/repos/diannyguarin/labs-backend/statuses/$GIT_COMMIT?access_token=${TOKEN1}${TOKEN2}" \
                  -H "Content-Type: application/json" \
                  -X POST \
                  -d "{\"state\": \"$BUILD_STATUS\",\"context\": \"continuous-integration/jenkins\", \"description\": \"Jenkins\", \"target_url\": \"$BUILD_URL\"}"
            '''
        }
    }
}
