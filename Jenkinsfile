pipeline {
    agent any

    environment {
        CONTAINER_NAME = "myapp"
        DATABASE_NAME = "wordpress"
        DATABASE_PASSWORD = "password"
        DATABASE_ROOT_PASSWORD = "root_password"
        DATABASE_USER = "user"
    }

    stages {
        stage('Checkout') {
            steps {
                // Lấy mã nguồn từ repository
                git credentialsId: 'b44b7680-1948-4b44-bea7-2b11f104c117', url: 'https://github.com/doanau1711/wordpress-local.git', branch: 'main'
            }
        }

        stage('Build and Deploy') {
            steps {
                script {
                    // Xây dựng và triển khai bằng Docker Compose
                    bat 'docker-compose down'

                    // Chỉ khởi động lại dịch vụ WordPress
                    bat 'docker-compose up --build'
                }
            }
        }

        stage('Post-deploy Cleanup') {
            steps {
                // Bất kỳ bước dọn dẹp nào sau triển khai, nếu cần
                bat 'docker system prune -f'
            }
        }
    }

    post {
        always {
            // Luôn luôn lưu trữ log
            archiveArtifacts artifacts: '**/logs/**', allowEmptyArchive: true
        }
    }
}
