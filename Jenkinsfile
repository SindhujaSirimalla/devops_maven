pipeline { 
    agent any 
    environment {
        mvn_home = tool name: 'M3', type: 'maven'
        dockerrun = 'docker run -p 9090:8080 -d --name WebappContainer2 sailavanya/mydockerhub:webappFrmJenkins'
    }
    stages {
        stage('SCM-Checkout') { 
            steps { 
                git credentialsId: 'git-credentials', url: 'https://github.com/lavanyaVarry/MyFirstWebApp.git'  
            }
        }
        stage('Build'){
            steps {
                sh "'${mvn_home}/bin/mvn' clean package"
            }
        }
        stage('Build-Docker-Image') {
            steps {
                sh "docker build -t sailavanya/mydockerhub:webappFrmJenkins ."
            }
        }
        stage('Push-Image-To-Hub'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pwd', usernameVariable: 'username')]) {
                    sh "docker login -u '${username}' -p '${pwd}'"
                    sh "docker push sailavanya/mydockerhub:webappFrmJenkins"
                }
            }
        }
        stage('Deploy-To-Remote-Server'){
            steps{
                sshagent(['webappserver']) {
                    sh "ssh -o StrictHostKeyChecking=no webserver@webappserver.centralus.cloudapp.azure.com ${dockerrun}"
                }
            }
        }
    }
}