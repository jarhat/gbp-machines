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
                    def directories = getDirectories("$WORKSPACE")
                    echo "$directories"
                    def fileName = "${env.JOB_BASE_NAME}/repos"
                    def repos = readFile(file: fileName).split("\n")
                    for (String repo: repos) {
                        copyArtifacts(projectName: "repos/${repo}")
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

@NonCPS
def getDirectories(path) {
    def dir = new File(path)
    def dirs = []
    dir.traverse(type: groovy.io.FileType.DIRECTORIES, maxDepth: -1) { d ->
        dirs.add(d) 
    }
    return dirs
}
