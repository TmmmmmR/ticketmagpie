pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'mvn clean install'
            }        
        }
        stage("SonarQube analysis") {
            steps {
                withSonarQubeEnv('sonar') {
                        sh 'mvn sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=a17ed4beaffbc0409c4b8f42c00a7c35f98656bc'
                }
            }
        }
        
        stage('Deliver for development') {
            when {
                branch 'development' 
            }
            steps {
                input message: 'Deploy for development ? (Click "Proceed" to continue)'
                sh 'Deploying to development ...'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'  
            }
            steps {
                input message: 'Deploy to production ? (Click "Proceed" to continue)'
                sh 'Deploying to production ...'
            }
        }
        
    }
}
