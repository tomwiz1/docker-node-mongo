node {
    checkout scm
    def customImage = docker.build("tomwi/nodejs_app:${env.BUILD_NUMBER}")
    customImage.push()
}

pipeline {
  agent {
    kubernetes {
      //cloud 'kubernetes'
      serviceAccount 'jenkins'
      containerTemplate {
        name 'helm-pod'
        image 'alpine/helm:3.1.1'
        ttyEnabled true
        command 'cat'
      }
    }
  }
  stages {
      stage('Run Helm') {
          steps {
              container('helm-pod'){
                  git (
                    url: 'git@github.com:tomwiz1/node-mongo-app.git',
                    branch: 'main',
                    credentialsId: 'jenkins-github-ssh'
                    cre
                  ) 
                  sh 'helm upgrade example-chart ./ --set=image.tag=${env.BUILD_NUMBER}'
              }

          }
      }
  }
}