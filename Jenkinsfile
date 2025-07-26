pipeline {
    agent any
    triggers {
       // pollSCM('* * * * *')  // Проверка изменений каждую минуту
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/ACKAPOB/B115.git'
                // credentialsId: 'ваш-токен' // Раскомментируйте для приватного репозитория
            }
        }
        stage('Execute SQL') {
            steps {
                sh '''
                    echo "Выполняем запросы..."
                    
                    # Создаем временные файлы для результатов
                    SIMPLE_RESULT="output_simple_query.sql.txt"
                    RAT_RESULT="output_rat_rna_query.sql.txt"
                    
                    # Выполняем запросы и сохраняем вывод
                    mysql --user=rfamro --host=mysql-rfam-public.ebi.ac.uk --port=4497 Rfam -e "$(cat simple_query.sql)" > ${SIMPLE_RESULT}
                    mysql --user=rfamro --host=mysql-rfam-public.ebi.ac.uk --port=4497 Rfam -e "$(cat rat_rna_query.sql)" > ${RAT_RESULT}
                    
                    # Проверяем результаты
                    echo "=== Результаты запросов ==="
                    cat ${SIMPLE_RESULT}
                    cat ${RAT_RESULT}
                    
                    echo "Размеры файлов:"
                    ls -lah *.txt
                '''
                archiveArtifacts artifacts: 'output_*.txt'
            }
        }
    }
}
