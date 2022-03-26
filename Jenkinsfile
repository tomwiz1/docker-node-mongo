node {
    checkout scm
    def customImage = docker.build("tomwi/nodejs_app:${env.BUILD_NUMBER}")
    customImage.push()
}

pipeline {
  agent {
      docker { image: 'alpine/helm:3.1.1' }
    }
  }
  stages {
      stage('Run Helm') {
          steps {
                  git url: 'git@github.com:tomwiz1/node-mongo-app.git', branch: 'main'
                  sh 'cd node-mongo-app'
                  sh 'helm upgrade example-chart ./ --set=image.tag=${env.BUILD_NUMBER}'
              }

          }
      }
  }
}


