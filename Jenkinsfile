pipeline {
    agent any

    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'Testing..'
                sh 'cd webapp && sudo docker run  --rm -e SONAR_HOST_URL="http://18.220.221.186:9000" -e SONAR_LOGIN="sqp_f4e907ce3980235c0e65405c2b9a739a2f5f8e82"  -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'cd webapp && npm install && npm run build'
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}