pipeline {
    agent any

       stages {
        stage('Sonar Analysis') {
            steps {
                echo 'Analyze Code..'
		sh 'cd webapp && sudo docker run  --rm -e SONAR_HOST_URL="http://52.63.135.203:9000" -e SONAR_LOGIN="sqp_ac82eb71e83dd1ec249dd23ec0cfee66b397caec"  -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
		}
             }
         
        stage('Build LMS') {
            steps {
                echo 'Building..'
                sh 'cd webapp && npm install && npm run build'
		}
             }

          }
      }
