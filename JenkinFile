pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    stages {
        stage('GIT') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ebtihel1/Devops.git'
            }
        }

        stage('Compile Stage') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'sonarqube-token', usernameVariable: 'SONAR_USER', passwordVariable: 'SONAR_TOKEN')]) {
                        sh """
                            mvn sonar:sonar \
                                -Dsonar.host.url=http://192.168.33.10:9000 \
                                -Dsonar.login=${SONAR_TOKEN} \
                                -Dmaven.test.skip=true
                        """
                    }
                }
            }
        }

        stage('docker image Stage') {
            steps {
                sh 'docker build -t timesheet:1.0.0 https://github.com/ebtihel1/Devops.git'
            }
        }
    }
}
