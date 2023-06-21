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
                script {
                     echo 'Releasing..'
                     def packageJSON = readJSON file: 'webapp/package.json'
                     def packageJSONVersion = packageJSON.version
                     echo "${packageJSONVersion}"
                     sh "zip webapp/dist-${packageJSONVersion}.zip -r webapp/dist"
                     sh "curl -v -u admin:Rushikesh@32 --upload-file webapp/dist-${packageJSONVersion}.zip http://18.220.221.186:8081/repository/lms/"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                def packageJSON = readJSON file: 'webapp/package.json'
                def packageJSONVersion = packageJSON.version
                echo "${packageJSONVersion}"
                sh "curl -u admin:Rushikesh@32 -X GET 'http://18.220.221.186:8081/repository/lms/dist-${packageJSONVersion}.zip\' --output dist-'${packageJSONVersion}'.zip"
                sh 'sudo rm -rf /var/www/html/*'
                sh "sudo unzip -o dist-'${packageJSONVersion}'.zip"
                sh "sudo cp -r webapp/dist/* /var/www/html"
            }
        }
    }
}