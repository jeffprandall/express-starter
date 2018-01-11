node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("jeffprandall/starter-express")
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }

    // stage('Host pulling latest container') {
        
    //     // stop the current container
    //     sh "ssh ${rancher1}@${server} docker stop ${app}"
    //     // download the newest version
    //     sh "ssh user@${server} docker pull ${app}"
    //     // start the new container
    //     sh "ssh user@${server} docker run ${app}"
    // }
    stage('Stop current container') {
        docker.withServer('tcp://localhost:2376') {
            docker.stop(app)
        }
    }

    stage('Pull the latest image') {
        docker.withServer('tcp://localhost:2376') {
            docker.build("app:latest")
        }
    }

    stage('Start updated container') {
        docker.withServer('tcp://localhost:2376') {
            docker.image(app)
        }
    }
}