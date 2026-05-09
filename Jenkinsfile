pipeline {
    agent any

    stages {

        stage('Initialize') {
            steps {
                echo 'Initializing pipeline...'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }

            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Deliver') {
            steps {
                sh 'chmod +x jenkins/scripts/deliver.sh'
                sh './jenkins/scripts/deliver.sh'
            }
        }

        stage('Deploy-To-Tomcat') {
            steps {
                sshagent(['tomcat']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no target/*.jar root@44.200.226.238:/opt/tomcat/webapps/
                    '''
                }
            }
        }
    }

    post {

        success {
            echo 'Pipeline executed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
