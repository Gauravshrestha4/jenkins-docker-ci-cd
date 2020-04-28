node {

    stage('Clone repository') {
        checkout scm
    }

    stage('Build Image') {
      steps {
        sh "docker build -t sample-app:${BUILD_NUMBER} ."
        sh "docker tag sample-app:${BUILD_NUMBER} <docker_username>/sample-app:${BUILD_NUMBER}"
      }
    }
	
     stage('Push Docker Image to Registry') {
       steps {
         withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USER')]) {
           sh "docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}"
         }
         sh "docker push <docker_username>/sample-app:${BUILD_NUMBER}"
        }
    }

    stage('Deploy image') {
	   
        // Stop the running container
        sh 'ssh -o StrictHostKeyChecking=No root@your_host_ip docker stop sample-container'
            
        // Remove the running container   
        sh 'ssh -o StrictHostKeyChecking=No root@your_host_ip docker rm sample-container'
        
	// Docker Login
	withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USER')]) {
           sh "ssh -o StrictHostKeyChecking=No root@your_host_ip docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}"
        }
	    
        // Pull latest the image
        sh 'ssh -o StrictHostKeyChecking=No root@your_host_ip docker pull <docker_username>/sample-app:${BUILD_NUMBER}'
	    
        // Run the container
        sh 'ssh -o StrictHostKeyChecking=No root@your_host_ip docker run -d --name sample-container -p 80:80 --restart=always <docker_username>/sample-app:${BUILD_NUMBER}'    
    }

    post {
      always {
        deleteDir()
      }
    }
}
