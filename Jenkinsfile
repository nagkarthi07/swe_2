//This file will create a CI/CD pipeline for building and deploying the docker image to k8s cluster using Github as source.
pipeline{

    environment {
registry = "akhpng31/phani645"
registryCredential = "docker"
def dateTag = new Date().format("yyyyMMdd-HHmmss")

// def dateTag = "latest"
}
agent any
stages{
stage('Building war’) {
steps {
script {
sh ‘rm -rf *.war'
sh ‘jar -cvf phani.war -C src/main/webapp .'
docker.withRegistry('',registryCredential){
def img = docker.build("akhpng31/phani645: + dateTag)
}
}
}
}
stage('Pushing latest code to Docker Hub') {
steps {
script {
docker.withRegistry('',registryCredential) {
def image = docker.build(’akhpng31/phani645:'+ dateTag, '. --no-cache')
docker.withRegistry('',registryCredential) {
image. push()
}
}
}
}
}
stage( ‘Deploying to single node in Rancher and load Balancer') {
steps {
script{
sh 'kubectl set image deployment/deployl container-0=akhpng31/phani645: '+date
sh 'kubectl set image deployment/deploy-1b container-@=akhpng31/phani645: '+dateTag
}
}
}
