node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {	    
	sh 'docker build -t sample-app .'
	sh 'docker save -o sample-app.tar'	    
    }

    stage('Push image') {
	// Push the image
	sh 'scp sample-app.tar root@example.com:/root'
	// Stop the running container
	// Remove the running container
	// Remove the current image
	// Load the new image
	// Run the container
    }

    stage('Remove image from Jenkins') {
	    
    }
}
