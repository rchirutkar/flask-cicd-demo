pipeline {
    // agent any //Run this pipeline on any available Jenkins agent. Since we're using a single Jenkins instance, it will run inside our Jenkins Docker container.

    agent {
        docker {
            image 'python:3.12'
        }
    }

    // environment {
    //     VENV = "venv" //nstead of hardcoding venv everywhere, we define it once. This makes the pipeline easier to maintain.
    // }

    stages {

        stage('Checkout') { //This clones our GitHub repository into the Jenkins workspace.

            steps {
                echo 'Checking out source code...'
                checkout scm //Without this step, Jenkins has no source code to build.
            }
        }

//This stage:
// Creates a Python virtual environment.
// Activates it.
// Upgrades pip.
// Installs all dependencies from requirements.txt.
// This satisfies the Build requirement of the assignment.
        stage('Build') {
            steps {
                echo 'Installing dependencies...'

                sh '''
                    python -m venv venv
                    . venv/bin/activate

                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }
// Runs the unit tests using pytest. This satisfies the Test requirement of the assignment.
        stage('Test') {
            steps {
                sh '''
                    . venv/bin/activate
                    python -m pytest -v
                '''
            }
        }
    }

    post {
        success {
            echo "Build completed successfully."
        }

        failure {
            echo "Build failed."
        }
    }
}