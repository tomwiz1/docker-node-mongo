node {
    checkout scm
    def customImage = docker.build("tomwi/nodejs_app:stable")
    customImage.push()
}
