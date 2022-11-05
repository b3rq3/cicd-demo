// Integration
// Used Jenkins plugins:
// - Docker Pipeline build
pipeline {
    agent 'any'
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'master', url: "https://github.com/igor-baiborodine/vaadin-demo-bakery-app.git"
            }
        }
        
        stage('Test') {
            steps {
                sh(script: 'mvn --batch-mode -Dmaven.test.failure.ignore=true test')
            }
        }
        
        stage('Build') {
            steps {
                script {
                    app = docker.build("bakery-app")
                }
                
            }
        }
        
        stage('Deploy') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                echo 'Deploy mockup'
            }
        }
    }
    post {
        always {
            junit(testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true)
        }
        failure {
            //mail to: foobar@example.lan, subject: 'Demo1 Pipeline failed'
            echo 'Oh noo! Demo pipeline failed'
        }
    }
}