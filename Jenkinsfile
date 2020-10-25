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
    }
}
