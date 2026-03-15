pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '✅ 代码已从 GitHub 拉取到工作空间！'
                sh 'pwd'
                sh 'ls -l'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // ✅ 修改：使用双引号 "..." 以便 ${BUILD_ID} 能被正确替换为数字
                    def imageTag = "devops-demo:${BUILD_ID}"
                    echo "Building image: ${imageTag}"
                    sh "docker build -t ${imageTag} ."
                }
            }
        }

        stage('Tag Latest') {
            steps {
                // ✅ 修改：同样使用双引号
                sh "docker tag devops-demo:${BUILD_ID} devops-demo:latest"
            }
        }

        stage('Deploy to K3s') {
            steps {
                echo '🚀 开始部署到 K3s 集群...'
                
                // ✅ 关键修改：
                // 1. 使用三单引号 ''' 包裹多行命令
                // 2. 第一行 export KUBECONFIG 指定配置文件路径
                // 3. 使用 ./deployment.yaml (相对路径) 代替绝对路径，更稳健
                sh '''
                    export KUBECONFIG=/var/lib/jenkins/.kube/config
                    kubectl apply -f ./deployment.yaml
                    kubectl rollout status deployment/devops-app --timeout=60s
                '''
            }
        }
    }

    post {
        always {
            echo '🏁 流水线执行结束'
        }
        success {
            echo '🎉 部署成功！'
        }
        failure {
            echo '❌ 部署失败，请检查日志。'
        }
    }
}
