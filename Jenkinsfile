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
                    curl "https://api.github.com/repos/diannyguarin/labs-backend/statuses/$GIT_COMMIT?access_token=a7e1728ab5ade8a69443eeae7be40ab0eff5db8c" \
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
                curl "https://api.github.com/repos/diannyguarin/labs-backend/statuses/$GIT_COMMIT?access_token=a7e1728ab5ade8a69443eeae7be40ab0eff5db8c" \
                  -H "Content-Type: application/json" \
                  -X POST \
                  -d "{\"state\": \"$BUILD_STATUS\",\"context\": \"continuous-integration/jenkins\", \"description\": \"Jenkins\", \"target_url\": \"$BUILD_URL\"}"
            '''
        }
    }
}
