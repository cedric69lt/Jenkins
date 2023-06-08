pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/cedric69lt/Jenkins']])
          }
       }
       stage('Build') {
            steps {
              sh 'docker build -t cedric69/nginx_umage2 .'
          }
       }
       stage('Push') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push cedric69/nginx_umage2:latest'
         }
        }
    }
    stage('Pull') {
            steps {
              sshPublisher(publishers: [sshPublisherDesc(configName: 'VmDockerCedric', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker run -d -p 80:80 --name conteneurnginx cedric69/nginx_umage2:latest', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*.jar')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
          }
       }
 }
