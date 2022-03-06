node {
    def app

    stage('Cloning repository') {
        checkout scm
    }

    stage('Building image') {
        app = docker.build("anuja9431/helloworld")
    }

    stage('Testing image') {

        app.inside {
            sh 'echo "Test is successful"'
        }
    }

    stage('Pushing image to Docker Hub') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Triggering Manifest-Update') {
        echo "triggering manifest job"
        build job: 'Update-Manifest-Job', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
}