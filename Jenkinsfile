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
                        sh 'mvn sonar:sonar -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=7c64d75fbcf46563e30a37a65f6fcbee2ccb6284'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                sh 'sleep 5s'
                timeout(time: 5, unit: 'MINUTES') {
                    script {
                        def result = waitForQualityGate()
                        if (result.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${result.status}"
                        } else {
                            echo "Quality gate passed with result: ${result.status}"
                        }
                    }
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
