pipeline {
    agent { label 'ubuntuserver' }

    stages {
        stage('install apache2') {
            steps {
                sh 'apt install apache2 -y'
            }
        }
        stage('test apache2') {
            steps {
                sh 'systemctl status apache2'
            }
        }
        stage('check log') {
            steps {
                script {
                    sh 'grep -E \'HTTP/1.[01]" [45][0-9][0-9]\' /var/log/apache2/access.log > errors.log || true'
                    
                    def errorsFound = sh(script: "test -s errors.log", returnStatus: true) == 0
                    if (errorsFound) {
                        echo '4xx and 5xx errors found in the log file:'
                        sh 'cat errors.log'
                    } else {
                        echo 'No 4xx or 5xx errors found in the log file.'
                    }
                }
            }
        }
    }
}
