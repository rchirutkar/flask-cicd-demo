pipeline {
    // agent any //Run this pipeline on any available Jenkins agent. Since we're using a single Jenkins instance, it will run inside our Jenkins Docker container.

    agent {
        docker {
            image 'python:3.12'
        }
    }

    environment {
        VENV = "venv" //nstead of hardcoding venv everywhere, we define it once. This makes the pipeline easier to maintain.
        HOME = "WORKSPACE" //Set the HOME environment variable to the Jenkins workspace. This is important for Python virtual environments.
    }

    stages {

        stage('Checkout') { //This clones our GitHub repository into the Jenkins workspace.

            steps {
                echo 'Checking out source code...'
                checkout scm //Without this step, Jenkins has no source code to build.
            }
        }

//This stage:
// Upgrades pip.
// Installs all dependencies from requirements.txt.
// This satisfies the Build requirement of the assignment.
        stage('Build') {
            steps {
                echo 'Installing dependencies...'

                sh '''
                    python3 -m pip install --upgrade pip
                    python3 -m pip install -r requirements.txt
                '''
            }
        }
// Runs the unit tests using pytest. This satisfies the Test requirement of the assignment.
        stage('Test') {
            steps {
                sh '''
                    export PYTHONPATH=$WORKSPACE
                    python3 -m pytest -v
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