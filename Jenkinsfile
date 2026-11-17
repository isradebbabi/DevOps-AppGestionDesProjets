pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Récupération du projet depuis GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/isradebbabi/DevOps-AppGestionDesProjets.git'
            }
        }

        stage('Tests unitaires') {
            steps {
                dir('backend') {
                    sh 'mvn clean test'
                }
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'backend/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Création du livrable (target)') {
            steps {
                dir('backend') {
                    sh 'mvn package -DskipTests'
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: 'backend/target/*.jar', fingerprint: true
                }
            }
        }
    }
}
