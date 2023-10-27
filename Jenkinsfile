pipeline {
    environment {
        registry = "akhpng31/phani645"
        registryCredential = "docker"
        def dateTag = new Date().format("yyyyMMdd-HHmmss")
    }
    agent any
    stages {
        stage('Build WAR') {
            steps {
                script {
                    sh 'rm -rf *.war'
                    sh 'jar -cvf phani.war -C src/main/webapp .'
                    docker.withRegistry('', registryCredential) {
                        def img = docker.build("${registry}:${dateTag}")
                    }
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        def image = docker.build("${registry}:${dateTag}", '. --no-cache')
                        image.push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes Cluster') {
            steps {
                script {
                    sh "kubectl set image deployment/dp container-0=${registry}:${dateTag}"
                    sh "kubectl set image deployment/dplb container-0=${registry}:${dateTag}"
                }
            }
        }
    }
}
