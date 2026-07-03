pipeline {
    agent any //Run this pipeline on any available Jenkins agent. Since we're using a single Jenkins instance, it will run inside our Jenkins Docker container.

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
                echo 'Creating Python virtual environment...'

                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }
    }
}