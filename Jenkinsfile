pipeline {
    agent {
        label 'docker'
    }
    environment {
        VERSION_NUMBER = ""
        DOCKER_TAG = ""
        TOM_USER = credentials('gittom')
        REPO_ADDRESS = ""
        
    }
    stages {
        stage('git tag') {
            steps {
                script {
                    REPO_ADDRESS = checkout(scm).GIT_URL
                    echo REPO_ADDRESS
                    VERSION_NUMBER = sh (
                        script: 'docker run --rm -v "$WORKSPACE:/repo" gittools/gitversion:5.3.5-linux-alpine.3.10-x64-netcoreapp3.1 /repo /showvariable MajorMinorPatch',
                        returnStdout: true
                        ).trim()
                   DOCKER_TAG = "Tiger-" + VERSION_NUMBER
                   echo DOCKER_TAG
                   sh "git tag $DOCKER_TAG"
                   sh "git push https://tomhoangle1:$TOM_USER@github.com/tomhoangle/gitversioning.git refs/tags/$DOCKER_TAG"
                }
            }
        }
    }
     post { 
        always { 
            cleanWs()
        }
    }
}
