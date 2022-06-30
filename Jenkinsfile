def img
def projectName = 'simpleFlask'
def version = "0.0.${currentBuild.number}"
def dockerImageTag = "${projectName}:${version}"
pipeline {
    environment {
        registry = "kss7/python-jenkins" //To push an image to Docker Hub, you must first name your local image using your Docker Hub username and the repository name that you created through Docker Hub on the web.
        registryCredential = 'docker-hub-login'
        dockerImage = ''
    }
    agent any
    stages {
        stage('checkout') {
            steps {
                git 'https://github.com/rsu1999/SimpleFlaskUI.git'
            }
        }

        


        stage('Build Image') {
            steps {
                script {
                    img = registry + ":${env.BUILD_ID}"
                    println ("${img}")
                    dockerImage = docker.build("${img}")
                 
                }
            }
        }



        

        stage('Deploy Container To Openshift') {
          steps {
            sh "oc login https://linuxops-miss31.conygre.com:8443 --username admin --password admin --insecure-skip-tls-verify=true"
            sh "oc project ${projectName} || oc new-project ${projectName}"
            sh "oc delete all --selector app=${projectName} || echo 'Unable to delete all previous openshift resources'"
            sh "oc new-app ${dockerImageTag} -l version=${version}"
            sh "oc expose svc/${projectName}"
      }
    }


    }}
