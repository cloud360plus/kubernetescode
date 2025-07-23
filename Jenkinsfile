node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        script {
            app = docker.build("dockerid/test")
        }
    }

    stage('Test image') {
        script {
            app.inside {
                sh 'echo "Tests passed"'
            }
        }
    }

    stage('Push image') {
        script {
            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                app.push("${env.BUILD_NUMBER}")
            }
        }
    }

    stage('Trigger ManifestUpdate') {
        script {
            echo "Triggering updatemanifest job"
            build job: 'updatemanifest', parameters: [
                string(name: 'DOCKERTAG', value: "${env.BUILD_NUMBER}")
            ]
        }
    }
}
