pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')  // Проверять изменения каждую минуту (можно заменить на webhook)
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ваш-репозиторий.git'
            }
        }
        stage('Run SQL Queries') {
            steps {
                script {
                    def changedFiles = sh(script: "git diff --name-only HEAD HEAD~1", returnStdout: true).trim().split('\n')
                    for (file in changedFiles) {
                        if (file.endsWith('.sql')) {
                            echo "Executing ${file}..."
                            sh "mysql --user rfamro --host mysql-rfam-public.ebi.ac.uk --port 4497 --database Rfam < ${file} | tee output_${file}.txt"
                            archiveArtifacts artifacts: "output_${file}.txt"
                        }
                    }
                }
            }
        }
    }
}
