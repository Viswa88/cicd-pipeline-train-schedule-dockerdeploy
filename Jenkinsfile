pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image'){
            when {
                 branch 'master'
            }
            steps {
                script {
                app = docker.build("viswa88/train-schedule")
                app.inside {
                     sh 'echo $(curl localhost:8080)'
                }
            }
        }
      }
    stage('Push Docker Image') {
        when {
             branch 'master'
        }
        steps {
            script {
                docker.withRegistry('https://registry.hub.docker.com', 'Docker_Hub_Login') {
                    app.push("${env.BUILD.NUMBER}")
                    app.push("latest")
                   }
               }
            }
         }
     }
}
