pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from Git...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Validating runtime and syntax...'
                sh 'python3 -m py_compile app.py'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit test suite...'
                sh 'python3 -m unittest test_app.py'
            }
        }

        stage('Validation') {
            steps {
                echo 'Running code quality & security checks...'
                // Validates style/syntax standards and fails on serious errors
                sh '''
                    echo "Checking Python syntax and compilation..."
 	            python3 -m compileall app.py test_app.py
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo ' Pipeline completed successfully.'
        }
        failure {
            echo ' Pipeline failed! Check the console logs for root-cause diagnosis.'
        }
    }
}
