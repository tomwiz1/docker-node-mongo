node {
    checkout scm
    def customImage = docker.build("tomwi/nodejs_app:${env.BUILD_ID}")
    customImage.push()
}
