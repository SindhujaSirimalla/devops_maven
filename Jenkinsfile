pipeline { 
    agent any 
    environment {
        m3 = tool name: 'maven_home', type: 'maven'
        dockerrun = 'docker run -p 9090:8080 -d --name WebappContainer2 sindhuja1/sindhujasirimalla:webappFrmJenkins'
    }
    stages {
        stage('SCM-Checkout') { 
            steps { 
                git credentialsId: 'git-credentials', url: 'https://github.com/SindhujaSirimalla/devops_maven.git'  
            }
        }
        stage('Build'){
            steps {
                sh "'${m3}/bin/mvn' clean package"
            }
        }
        stage('Build-Docker-Image') {
            steps {
                sh "docker build -t sindhuja1/sindhujasirimalla:webappFrmJenkins ."
            }
        }
        stage('Push-Image-To-Hub'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockercredentials', passwordVariable: 'pwd', usernameVariable: 'username')]) {
                    sh "docker login -u '${dockerusername}' -p '${dockerpassword}'"
                    sh "docker push sindhuja1/sindhujasirimlla:webappFrmJenkins"
                }
            }
        }
        stage('Deploy-To-Remote-Server'){
            steps{
                sshagent(['webserver19']) {
                    sh "ssh -o StrictHostKeyChecking=no Sindhuja@webserver19.centralus.cloudapp.azure.com ${dockerrun}"
                }
            }
        }
    }
}