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


        stage('Release LMS') {
            steps {
                script {
		    echo "Releasing"
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"
		    sh "zip webapp/dist-${packageJSONVersion}.zip -r webapp/dist"
		    sh "curl -v -u admin:Sathish@1989 --upload-file webapp/dist-${packageJSONVersion}.zip http://52.63.135.203:8081/repository/lms/"

               }
            }
         }
      
        stage('Deploy LMS') {
            steps {
                script {
                    echo "Deploying.."       
                    def packageJSON = readJSON file: 'webapp/package.json'
                    def packageJSONVersion = packageJSON.version
                    echo "${packageJSONVersion}"  
                    sh "curl -u admin:Sathish@1989 -X GET \'http://52.63.135.203:8081/repository/lms/dist-${packageJSONVersion}.zip\' --output dist-'${packageJSONVersion}'.zip"
                    sh 'sudo rm -rf /var/www/html/*'
                    sh "sudo unzip -o dist-'${packageJSONVersion}'.zip"
                    sh "sudo cp -r webapp/dist/* /var/www/html"
               }
            }
         }
      }
      }
      }
