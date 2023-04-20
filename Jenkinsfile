node {
    def app
    
    stage('Initialize')
    {
        env.PATH = "/usr/local/bin:${env.PATH}"
    }

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("dsiva/test")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub1') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest1', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
