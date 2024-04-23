pipeline {
    agent any

    tools {
        maven "maven3"
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
    }
}