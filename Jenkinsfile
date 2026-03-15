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
                    echo "Building image devops-demo:${BUILD_ID}"
                    // ⚠️ 注意：这里单引号会导致 ${BUILD_ID} 不会被替换，变成 literal 字符串
                    // 如果构建失败，请把单引号改为双引号："docker build ..."
                    sh 'docker build -t devops-demo:${BUILD_ID} .'
                }
            }
        }

        stage('Tag Latest') {
            steps {
                sh 'docker tag devops-demo:${BUILD_ID} devops-demo:latest'
            }
        }

        // ✅ 修复：把这个 stage 移回 stages 的大括号里面
        stage('Deploy to K3s') {
            steps {
                echo 'Deploying to Kubernetes...'
                sh 'kubectl apply -f /root/CI-CD-test/deployment.yaml'
                sh 'kubectl rollout status deployment/devops-app --timeout=60s'
            }
        } 
    } // ✅ 修复：stages 在这里统一闭合

    post {
        always {
            echo '🏁 流水线执行结束'
        }
    }
}
