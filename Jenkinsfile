pipeline {
    agent any
    
    stages {
        stage('Execute SQL') {
            steps {
                script {
                    // 1. Простейший запрос из файла
                    sh '''
                        echo "Выполняем simple_query.sql:"
                        cat simple_query.sql
                        mysql --user=rfamro --host=mysql-rfam-public.ebi.ac.uk --port=4497 Rfam \
                             -e "$(cat simple_query.sql)" > simple_result.txt 2>&1 || echo "Ошибка выполнения simple_query.sql" > simple_result.txt
                    '''
                    
                    // 2. Запрос по крысам из файла
                    sh '''
                        echo "Выполняем rat_rna_query.sql:"
                        cat rat_rna_query.sql
                        mysql --user=rfamro --host=mysql-rfam-public.ebi.ac.uk --port=4497 Rfam \
                             -e "$(cat rat_rna_query.sql)" > rat_result.txt 2>&1 || echo "Ошибка выполнения rat_rna_query.sql" > rat_result.txt
                    '''
                    
                    // Сохраняем результаты
                    archiveArtifacts artifacts: '*_result.txt'
                    
                    // Выводим краткий отчет
                    sh '''
                        echo "=== Результаты выполнения ==="
                        echo "simple_query.sql:"
                        cat simple_result.txt
                        echo ""
                        echo "rat_rna_query.sql:"
                        cat rat_result.txt
                    '''
                }
            }
        }
    }
}
