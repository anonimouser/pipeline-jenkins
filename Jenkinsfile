pipeline {
    agent {
        node {
            // https://jenkins.test/computer/(built-in)/configure
            label 'docker-enabled'
        }
    }

    environment {
        // https://hub.docker.com/repositories
        imageName = "anonimouser/pipeline-jenkins"

        // https://hub.docker.com/settings/security
        registryCredential = 'docker-hub'

        publicPort = "9003"
    }

    stages {
        stage('Test') {
            steps {
                sh 'echo Fake test'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build(imageName, "-f Dockerfile .")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    // Primer parámetro en blanco -> Docker Hub, si no, URL del registro privado
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Docker Run') {
            steps {
                sh "docker rm -f nginx-egibide-9003"
                sh "docker run -d -p ${publicPort}:80 --name nginx-egibide-9003 ${imageName}:latest"
            }
        }
    }
}
