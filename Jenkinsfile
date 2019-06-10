pipeline {
    agent any

    options {
	timeout(time: 1, unit: 'HOURS') 
    }
	
	environment {	
	registryCredential = 'dockerhub'
    	APP_NAME = 'dotnetcore_app'
    	BUILD_NUMBER = "${env.BUILD_NUMBER}"
    	IMAGE_VERSION="v_${BUILD_NUMBER}"
	LOCAL_PORT='9000'
	APP_PORT='80'
	DOCKER_USER_N='pisutp'
    //GIT_URL="git@github.yourdomain.com:mpatel/${APP_NAME}.git"
    //GIT_CRED_ID='izleka2IGSTDK+MiYOG3b3lZU9nYxhiJOrxhlaJ1gAA='
    //WS_PRODUCT_TOKEN='FJbep9fKLeJa/Cwh7IJbL0lPfdYg7q4zxvALAxWPLnc='
    //WS_PROJECT_TOKEN='zwzxtyeBntxX4ixHD1iE2dOr4DVFHPp7D0Czn84DEF4='
    //HIPCHAT_TOKEN = 'SpVaURsSTcWaHKulZ6L4L+sjKxhGXCkjSbcqzL42ziU='
    //HIPCHAT_ROOM = 'NotificationRoomName'
	}

    stages {

        stage("Build images") {
            steps {               
		sh "docker build -t ${APP_NAME}:${IMAGE_VERSION} ."
		sh "docker images | grep ${APP_NAME}"               
            }
        }
		
	stage("Test") {
            steps {
                sh "docker run --name ${APP_NAME} -p ${LOCAL_PORT}:${APP_PORT} -d ${APP_NAME}:${IMAGE_VERSION}"
		sh "sleep 10"
                sh "curl -v 23.96.105.95:${LOCAL_PORT}"
		sh "docker stop ${APP_NAME}"
            }
        }
		
	stage("Docker Push Images") {
            steps {
		//docker.withRegistry( '', registryCredential )
		//sh "docker login -u ${DOCKER_USER_N} -p ${DOCKER_USER_P}"
		sh "docker tag ${APP_NAME}:latest ${DOCKER_USER_N}/${APP_NAME}:${IMAGE_VERSION}"
		sh "docker push ${DOCKER_USER_N}/${APP_NAME}:${IMAGE_VERSION}"
		sh "docker push ${DOCKER_USER_N}/${APP_NAME}:latest"
            }
        }
		
	stage("Docker Clean Images & Containers") {
            steps {
		//sh "docker rm -v $(docker ps -aq -f status=exited)"
		sh "docker rmi ${DOCKER_USER_N}/${APP_NAME}:${IMAGE_VERSION} ${DOCKER_USER_N}/${APP_NAME}:latest"
		//sh "docker rmi $(docker images | grep "^<none>" | awk "{print $3}") || true"
		sh "docker images | grep ${APP_NAME} | grep ${IMAGE_VERSION}"
            }
        }
		
	stage("Deploy") {
            steps {
		//docker.withRegistry( '', registryCredential )
                sh "docker pull ${DOCKER_USER_N}/${APP_NAME}:${IMAGE_VERSION}"
		sh "docker run --name ${APP_NAME} -p ${LOCAL_PORT}:${APP_PORT} -d ${DOCKER_USER_N}/${APP_NAME}:${IMAGE_VERSION}"
            }
        }

}
}
