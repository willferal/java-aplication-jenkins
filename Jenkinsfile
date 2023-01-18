pipeline{
    agent any
    
    stages{
        stage('Build Backend'){
            steps{
                sh 'mvn clean package -DskipTests=true'
            }
        }

        stage('Deploy Backend'){
            steps{
                sh 'echo $PWD'
                deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend' , war: 'target/tasks-backend.war'
            }
        }

        stage('Deploy Frontend'){
            steps{
                sh 'echo deploy frontend'
                dir('frontend'){
                    git credentialsId: 'Github-Account', url: 'http://github.com/willferal/tasks-frontend'
                    sh 'mvn clean package'
                    sh 'echo $PWD'
                    deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
        
        stage('Deploy prod'){
            steps{
                sh 'docker-compose build'
                sh 'docker-compose up -d'
            }
        }
    }
}