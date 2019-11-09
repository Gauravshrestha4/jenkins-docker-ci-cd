node {

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {	    
	sh 'docker build -t sample-app .'
	sh 'docker save -o sample-app.tar sample-app:latest'	    
    }

    stage('Push image') {
	    
	// Push the image
	sh 'scp -o StrictHostKeyChecking=No sample-app.tar root@139.162.88.122:/root'
	
	// Stop the running container
	sh 'ssh -o StrictHostKeyChecking=No root@139.162.88.122 docker stop sample-container'
	    
	// Remove the running container   
	sh 'ssh -o StrictHostKeyChecking=No root@139.162.88.122 docker rm sample-container'
	
	// Remove the current image 
	sh 'ssh -o StrictHostKeyChecking=No root@139.162.88.122 docker rmi sample-app'
	    
	// Load the new image
	sh 'ssh -o StrictHostKeyChecking=No root@139.162.88.122 docker load -i sample-app.tar'
	    
	// Run the container
	sh 'ssh -o StrictHostKeyChecking=No root@139.162.88.122 docker run -d --name sample-container -p 80:80 --restart=always sample-app'    
    }

    stage('Remove image from Jenkins') {
	  sh 'rm sample-app.tar'  
	  sh 'docker rmi sample-app'  
    }
}
