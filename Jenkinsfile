pipeline {
    agent any 
    stages {
        stage ("Installation Checkup") {
            steps {
                echo "Checking if python3 & pip3 are installed or not ...."
                script {
                    def installations = sh(script: "command -v python3 && command -v pip3", returnStatus: true) == 0
                    def pythonInstalled = installations
                    def pipInstalled = installations
                    if (!installations) {
                        echo "python3 or pip3 is not installed. Installing missing packages..."
                        sh "sudo apt-get update && sudo apt-get install -y python3 python3-pip"
                    } else {
                        echo "python3 and pip3 are already installed."
                        }
                    }
                }
        }
        } // Close the Installation Checkup stage
        stage ("Install Dependencies") {
            steps {
                echo "Installing Dependencies...."
                script {
                    if (fileExists('requirements.txt')) {
                        echo "requirements.txt found. Installing dependencies..."
                        sh "pip3 install -r requirements.txt"
                    } else {
                        echo "requirements.txt not found. Skipping dependency installation."
                    }
                }
            }   
         }
        stage ("Run Tests") {
            steps {
                echo "Running Tests..."
                script {
                    if (fileExists('test_app.py')) {
                        echo "test_app.py found. Running tests..."
                        sh "python3 test_app.py"
                    } else {
                        echo "test_app.py not found. Skipping tests."
                    }
                }
            }
        }

    }
}
