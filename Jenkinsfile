pipeline{
    agent any
    tools{
        maven 'maven_3_9_9'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-credentials', url: 'https://github.com/Prajwal-career/todo-repo']])
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Build Docker image'){
            steps{
                script{
                    sh 'docker build -t project-todo-application:latest'
                }
            }
        }
        stage('Docker login'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
}
            }
            
        }
        
        stage('Push image'){
            steps{
                sh "docker push ${DOCKER_USERNAME}/project-todo-application:latest"
            }
        }
        stage('Run docker compose'){
            steps{
                script{
                    sh 'docker compose up'
                }
            }
        }
    }
}
