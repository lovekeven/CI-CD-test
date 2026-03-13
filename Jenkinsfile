pipeline {
    agent any

    stages {
        // ✅ 修改这里：让 Jenkins 自动使用当前拉取的代码，而不是写死的本地路径
        stage('Checkout') {
            steps {
                echo '✅ 代码已从 GitHub 拉取到工作空间！'
                sh 'pwd'          // 打印当前目录确认位置
                sh 'ls -l'        // 列出文件，确认 Dockerfile 是否存在
                // 不需要手动 git clone，Pipeline 模式会自动帮你做这一步
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // 注意：Jenkins 环境变量用 ${BUILD_ID} 不需要转义符 \
                    echo "Building image devops-demo:${BUILD_ID}"
                    sh 'docker build -t devops-demo:${BUILD_ID} .'
                }
            }
        }

        stage('Tag Latest') {
            steps {
                sh 'docker tag devops-demo:${BUILD_ID} devops-demo:latest'
            }
        }
    }
    
    post {
        always {
            echo '🏁 流水线执行结束'
        }
    }
}
