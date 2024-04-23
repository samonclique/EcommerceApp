pipeline {
    agent any

    tools {
        maven "maven3"
    }
    
     environment {
                ScannerHome = tool 'sonar5'
    }
    
    stages {
        stage('Git Clone') {
            steps {
                git 'https://github.com/samonclique/EcommerceApp.git'
            }
        }

        stage('Build and Test') {
            steps {
            // Specify the directory containing the pom.xml file
                dir('EcommerceApp') {
                    sh ''' 
                    echo "Build and testing.."
                    mvn clean test package
                    '''
                }
            }
        }
        stage('Sonarqube Analysis') {
            steps {
                echo "Analyzing with SonarQube.."
                script {
                    dir ('EcommerceApp') {
                        withSonarQubeEnv(credentialsId: 'jenkins-sonar-key') {
                            sh "${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=ecommerce-app -Dsonar.java.binaries=."
                        }
                    }
                }
            }
        }
        stage('Build & Push to Docker Registry') {
            steps {
                dir ('EcommerceApp') {
                    script {
                    withDockerRegistry(credentialsId: 'docker-jenkins', toolName: 'docker') {
                        sh "docker build -t samonclique/ecommerceapp:latest ."
                        sh "docker push samonclique/ecommerceapp:latest"
                        }
                    }
                }
            }
        }
    }
}