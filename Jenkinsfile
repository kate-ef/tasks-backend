pipeline {
    agent any
    stages {
        stage('Build Backend'){
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage('Unit Tests'){
            steps{
                bat 'mvn test'
            }
        }
        stage('Sonar Analysis'){
            environment {
                scannerHome = tool 'SONAR SCANNER'
            }
            steps{
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeplyBack -Dsonar.host.url=http://10.0.0.100:9000 -Dsonar.login=5c03adec957584f1986c497495189ea36dccb7ca -Dsonar.java.binaries=target"
                }
            }
        }
        stage('Deploy Backend'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'tomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage('Deploy Frontend'){
            steps{
                dir('frontend'){
                    git credentialsId: 'github_login', url: 'https://github.com/kate-ef/tasks-frontend.git'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'tomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'

                }
            }
        }
    }
}
https://github.com/kate-ef/tasks-frontend.git