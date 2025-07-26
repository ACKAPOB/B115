pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Автоматически использует текущий репозиторий
            }
        }
        
        stage('Execute SQL') {
            steps {
                sh '''
                    echo "=== Выполняем SQL-запросы из репозитория ==="
                    
                    # 1. Простейший запрос
                    echo "Запрос из simple_query.sql:"
                    cat simple_query.sql
                    mysql --user=rfamro --host=mysql-rfam-public.ebi.ac.uk --port=4497 Rfam < simple_query.sql > simple_result.txt 2>&1
                    
                    # 2. Запрос по крысам
                    echo "Запрос из rat_rna_query.sql:"
                    cat rat_rna_query.sql
                    mysql --user=rfamro --host=mysql-rfam-public.ebi.ac.uk --port=4497 Rfam < rat_rna_query.sql > rat_result.txt 2>&1
                    
                    # Проверяем результаты
                    echo "=== Результаты ==="
                    ls -la *.txt
                '''
                archiveArtifacts artifacts: '*.txt'
            }
        }
    }
}
