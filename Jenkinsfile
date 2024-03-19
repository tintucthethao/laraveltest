// git repository info
def gitRepository = 'https://bitbucket.baityapp.online/scm/bait/laravel_image.git'
def gitBranch = 'master'

// Image infor in registry
def imageGroup = 'hakensystem'
def appName = "baity_laravel"

// harbor-registry credentials
def registryCredential = 'dockerhub_leloimr'
// github credentials
def githubCredential = 'github_mrleloi'

dockerBuildCommand = './'
def version = "prod-0.${BUILD_NUMBER}"

pipeline {
    agent {
        kubernetes {
            label "jenkins-agent-dockernodejs"
        }
    }
    
    environment {
        DOCKER_IMAGE_NAME = "${imageGroup}/${appName}"
    }

    stages {
    
        stage('Checkout project') 
        {
          steps 
          {
            echo "checkout project"
            git branch: gitBranch,
               credentialsId: githubCredential,
               url: gitRepository
            sh "git reset --hard"
          }
        }
        stage('Build docker and push to registry') 
        {
          steps 
          {
            container('docker') {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME, dockerBuildCommand)
                    docker.withRegistry('', registryCredential ) {                       
                       app.push(version)
                    }
    
                    sh "docker rmi ${DOCKER_IMAGE_NAME}:${version} -f"				
                }
            }
          }
        }
    }
}
