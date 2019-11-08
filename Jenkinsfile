node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {	    
	sh 'docker build -t sample-app .'
	sh 'docker save -o sample-app.tar sample-app'	    
    }

    stage('Push image') {
	    
	// Push the image
	sh 'scp sample-app.tar root@example.com:/root'
	
	// Stop the running container & Remove the running container & Remove the current image
	sh 'ssh root@192.168.1.1 "docker stop sample-app && docker rm sample-app && docker rmi sample-app"'
	    
	// Load the new image
	sh 'ssh root@192.168.1.1 "docker load -i dynamo-frontend.tar"'
	    
	// Run the container
	sh 'ssh root@192.168.1.1 "docker run sample-app -d 80:80"'    
    }

    stage('Remove image from Jenkins') {
	  sh 'rm sample-app.tar'  
	  sh 'docker rmi sample-app'  
    }
}
