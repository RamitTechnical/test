pipeline {
    agent any

    stages {

        stage("build") {
            steps {
                echo "Building the application"
                git url: "https://github.com/RamitTechnical/test.git", branch: "master"
            }
        }

        stage("test") {
            steps {
                echo "Running Test"
                sh "docker build -t note-app ."
            }
        }

        stage("Push") {
            steps {
                echo "Pushing the application"
                withCredentials([
                    usernamePassword(
                        credentialsId: 'Docker-Hub',
                        usernameVariable: 'dockerHubUser',
                        passwordVariable: 'dockerHubPassword'
                    )
                ]) {
                    sh "docker tag note-app ${dockerHubUser}/note-app:latest"
                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
                    sh "docker push ${dockerHubUser}/note-app:latest"
                }
            }
        }

        stage("deploy") {
            steps {
                echo "Deploying Application"
                sh "docker compose down && docker compose up -d"
            }
        }

    }
}
