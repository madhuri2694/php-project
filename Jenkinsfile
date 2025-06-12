pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/madhuri2694/php-project/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t madhuri2694/newimage:v2 .'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push madhuri2694/newimage:v2'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f My-first-containe2211 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe2211 -p 8083:80 madhuri2694/newimage:v1'
                    sshagent(['sshkeypair']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe2111 -p 8083:80 madhuri2694/26mayimg:v1"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.80.85 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.80.85 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
