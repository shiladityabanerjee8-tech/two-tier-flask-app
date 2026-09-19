pipeline {

    agent any

    stages {

        stage("Code Clone") {
            steps {
                deleteDir()
                git url: "https://github.com/shiladityabanerjee8-tech/two-tier-flask-app.git",
                    branch: "master"
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "Running application test..."
                sh "docker image inspect two-tier-flask-app"
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {

                    sh 'echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin'

                    sh 'docker image tag two-tier-flask-app $dockerHubUser/two-tier-flask-app:latest'

                    sh 'docker push $dockerHubUser/two-tier-flask-app:latest'

                    sh 'docker logout'
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
