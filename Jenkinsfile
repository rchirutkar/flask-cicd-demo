pipeline {
    agent any //Run this pipeline on any available Jenkins agent. Since we're using a single Jenkins instance, it will run inside our Jenkins Docker container.

   //  agent {
   //      docker {
   //          image 'python:3.12'
			// //label 'Built-In Node' // Run this pipeline on the Jenkins agent. This is important because we want to ensure that the pipeline runs in a consistent environment, which is provided by the Jenkins agent.
   //      }
   //  }

    environment {
        VENV = "venv" //nstead of hardcoding venv everywhere, we define it once. This makes the pipeline easier to maintain.
        MONGO_URI = credentials('mongo-uri')
		SECRET_KEY = credentials('flask-secret')
    }

    stages {

        stage('Checkout') { //This clones our GitHub repository into the Jenkins workspace.

            steps {
                echo 'Checking out source code...'
				cleanWs()
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
					export HOME=$WORKSPACE
                    python3 -m pip install --upgrade pip
                    python3 -m pip install -r requirements.txt
                '''
            }
        }
// Runs the unit tests using pytest. This satisfies the Test requirement of the assignment.
        stage('Test') {
            steps {
                sh '''
					export HOME=$WORKSPACE
	            	export PYTHONPATH=$WORKSPACE
                    python3 -m pytest -v
                '''
            }
        }
// This stage tests the SSH connection to the EC2 instance. 
// It uses the SSH key stored in Jenkins credentials to connect to the EC2 instance 
// and run a simple command (hostname and whoami) to verify that the connection is successful.
        // stage('Test SSH') {
        //     steps {
        //         sshagent(credentials: ['ec2-ssh-key']) {
        //             sh '''
        //                 ssh -o StrictHostKeyChecking=no ubuntu@13.126.82.12 "hostname && whoami"
        //             '''
        //         }
        //     }
        // }
// This stage deploys the application to the EC2 instance. It uses SSH to connect to the EC2 instance, 
// pulls the latest code from GitHub, installs any new dependencies, and restarts the Flask application.
        stage('Deploy') {
            steps {
				// sshagent(credentials: ['ranjeet-ec2-ssh-key']) {
				//     ...
				// }
                sshagent(credentials: ['ranjeet-ec2-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@13.126.82.12 "
                            cd /home/ubuntu/apps/flask-cicd-demo &&

							git fetch origin
							
							git reset --hard origin/main
							
                            git pull origin main &&

                            source venv/bin/activate &&

							python -m pip install -r requirements.txt &&

                            pkill -f 'python app.py' || true &&

                            nohup python app.py > app.log 2>&1 < /dev/null &

							sleep 5 &&

                    		curl http://localhost:8000/health
                        "
                    '''
                }
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
