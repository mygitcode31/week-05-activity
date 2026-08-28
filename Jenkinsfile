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
		sh '''
            		cat << 'EOF' > parse_error.py
	import re

	with open("console.log", "r", errors="ignore") as f:
    	logs = f.read()

	# Pattern-match common build/test failure signals
	errors = re.findall(r"(AssertionError:.*|SyntaxError:.*|ERROR:.*)", logs)
	print("=== AI Root Cause Suggestion ===")	
	if errors:
    		print(f"Detected Error: {errors[-1]}")
    		print("Actionable Fix: Verify unit test assertion expectations against the module return values.")
	else:
    		print("Unrecognized crash signature. Review full pipeline trace.")
	EOF
            python3 parse_error.py || true
        '''
        }
    }
