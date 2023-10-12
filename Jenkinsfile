pipeline {
    agent any

    stages {
        stage('Fetch Code') {
            steps {
                echo 'fetch code from github'
                git branch: 'main', url: 'https://github.com/PranitRout07/Movie-recommender.git'
                echo 'code fetch successful'
            }
        }
        stage('Python Code Analysis'){
            environment {
                scannerHome = tool 'sonar4.7'
            }
            steps {
               withSonarQubeEnv('sonar') {
                   bat '%scannerHome%\\bin\\sonar-scanner -Dsonar.projectKey=1-- ' +
                    '-Dsonar.projectName=MOVIE-RECOMMENDER ' +
                    '-Dsonar.projectVersion=1.0 ' +
                    '-Dsonar.sources=. '
              }
            }
        }
        stage('Build docker image'){
            steps {
                bat 'docker build -t movie-recommender .'
            }
        }
        stage('Push Docker image to Docker Hub'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerPass', usernameVariable: 'dockerUser')]) {
                   bat "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                   bat "docker tag movie-recommender ${env.dockerUser}/movie-recommender:latest"
                   bat "docker push ${env.dockerUser}/movie-recommender:latest "
                }
            }
        }
        stage('Run on docker'){
            steps {
                echo "running docker image at port 8501"
                bat "docker run -d -p 8501:8501 pranit007/movie-recommender:latest"
            }
        }
        
    }
}
