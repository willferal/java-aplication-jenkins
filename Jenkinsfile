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
                deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/ta sks-backend.war'
            }
        }

        stage('Deploy Frontend'){
            steps{
                sh 'echo deploy frontend'
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'http://github.com/willferal/tasks-frontend'
                    sh 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
    }
}