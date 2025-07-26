pipeline {
    agent any
    
    stages {
        stage('Execute SQL') {
            steps {
                sh '''
                    # Просто выполняем оба запроса и сохраняем результаты
                    mysql --user=rfamro --host=mysql-rfam-public.ebi.ac.uk --port=4497 Rfam < simple_query.sql > simple_result.txt
                    mysql --user=rfamro --host=mysql-rfam-public.ebi.ac.uk --port=4497 Rfam < rat_rna_query.sql > rat_result.txt
                    
                    # Проверяем, что файлы созданы
                    ls -la *.txt
                '''
                archiveArtifacts artifacts: '*.txt'
            }
        }
    }
}
