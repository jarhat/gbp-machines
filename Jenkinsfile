pipeline {
    agent any

    environment {
        machine = "${env.JOB_BASE_NAME}"
        build = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('sync repos') {
            steps {
                script {
                    findSubFolder("${env.WORKSPACE}")

                    }
                }
            }
        }
        stage('build chroot') {
            steps {
                sh 'make chroot'
            }
        }
        stage('update world') {
            steps {
                sh 'make world'
            }
        }
        stage('build artifacts') {
            steps {
                sh 'make archive'
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'build.tar.gz'
            sh 'make push'
        }
    }
}

def findSubFolder(String base_folder) {
    dir(base_folder) {
        findFiles().findAll { item -> item.directory }
    }
}
