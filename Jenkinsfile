


def COLOR_MAP = ['SUCCESS': 'good', 'FAILURE': 'danger', 'UNSTABLE': 'danger', 'ABORTED': 'danger']

pipeline {
    agent any
    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
        withCredentials([usernamePassword(credentialsId: 'manish-git-cred',
                 usernameVariable: 'username',
                 passwordVariable: 'password')])
    }
    stages {
        stage('Build') {
            steps {
	            sh 'echo build '
            }
        }
        stage('Docker Build and push') {
            steps {
            sh
               sh "docker build -t manish8757/rancher:${GIT_COMMIT} . "
               sh "docker login -u manish8757 -p manish123@"
               sh "docker push manish8757/rancher:${GIT_COMMIT} "
            }
        }
        stage('Git Push'){
        steps{
        script{
            GIT_CREDS = credentials(manish-git-cred)
            sh '''
                git clone https://github.com/Manish7992/guestbook-v1-code.git
                cd guestbook-v1-code
                git pull https://${GIT_CREDS_USR}:${GIT_CREDS_PSW}@github.com/Manish7992/guestbook-v1-code.git
                sed ' s%manish8757/rancher:${GIT_PREVIOUS_SUCCESSFUL_COMMIT}%manish8757/rancher:${GIT_COMMIT} %' guestbook-deployment.yaml
                git add .
                git commit -m "manish8757/rancher:${GIT_COMMIT}"
                git push https://${GIT_CREDS_USR}:${GIT_CREDS_PSW}@github.com/Manish7992/guestbook-v1-code.git
            '''
             }
          }
        }
    }

}
