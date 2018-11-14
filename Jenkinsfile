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
                        sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Deliver for development') {
            when {
                branch 'development' 
            }
            steps {
                input message: 'Deploy for development ? (Click "Proceed" to continue)'
                //sh 'Deploying to development ...'
            }
        }
        
         stage('Quality Gate') {
            when {
                branch 'production' 
            }steps {
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
        
        stage('Deploy for production') {
            when {
                branch 'production'  
            }
            steps {
                input message: 'Deploy to production ? (Click "Proceed" to continue)'
                //sh 'Deploying to production ...'
            }
        }
        
    }
}
