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
        
        stage('Unit Tests') {
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
        
        stage('Copy image to staging') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'staging_ssh_key', keyFileVariable: 'keyfile')]){
                    sh '''
                        docker save bakery-app:latest | bzip2 | ssh -i ${keyfile} -o StrictHostKeyChecking=no vagrant@192.168.56.101 docker load
                    '''
                }
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