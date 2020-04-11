pipeline {
     agent {label 'jenkins-slave1'}
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
         steps {
             script{
                 app=docker.build("mazid1315/train-schedule")
                 app.inside{
                    sh 'echo $(curl 35.192.29.251:4243)'
                 }
             }
         }    
    }
        stage('Push Docker Image') {
              when {
                branch 'master'
            }
         steps {
             script{
                 docker.withRegistry('https://registry.hub.docker.com','docker_hub_login'){
                     app.push("${env.BUILD_NUMBER}")
                     app.push("latest")
                 }
             }   
         }  
     }
   }           
}           
