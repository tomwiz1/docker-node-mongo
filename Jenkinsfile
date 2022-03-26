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
                    
                  ) 
                  sh '''
                  ls
                  pwd
                  DEPLOYED=$(helm list | grep -E "example-chart" | grep DEPLOYED | wc -l)
                  if [ $DEPLOYED == 0 ] ; then
                     helm install example-chart node-mongo-app/ --namespace node-app
                  else 
                    helm upgrade example-chart node-mongo-app/ --set=image.tag=10 -n node-app
                  fi
                  echo "Deployed!"
                  '''
              }

          }
      }
  }
}