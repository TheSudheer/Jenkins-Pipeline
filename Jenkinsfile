pipeline {
    agent any 
    stages {
        stage ("Installation Checkup") {
            steps {
                echo "Checking if python3 & pip3 are installed or not ...."
                script {
                    def pythonInstalled = sh(script: "command -v python3", returnStatus: true) == 0
                    def pipInstalled = sh(script: "command -v pip3", returnStatus: true) == 0

                    if (!pythonInstalled) {
                        echo "python3 is not installed. Installing python3..."
                        sh "sudo apt-get update && sudo apt-get install -y python3"
                    } else {
                        echo "python3 is already installed."
                    }

                    if (!pipInstalled) {
                        echo "pip3 is not installed. Installing pip3..."
                        sh "sudo apt-get install -y python3-pip"
                    } else {
                        echo "pip3 is already installed."
                    }
                }
            }
        }
    }
}
