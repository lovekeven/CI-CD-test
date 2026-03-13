pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Using local Git repo at /root/devops-demo'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building image devops-demo:\${BUILD_ID}"
                    sh 'docker build -t devops-demo:\${BUILD_ID} .'
                }
            }
        }

        stage('Tag Latest') {
            steps {
                sh 'docker tag devops-demo:\${BUILD_ID} devops-demo:latest'
            }
        }
    }
}
EOF
echo "// Trigger new build to fix cache issue - $(date)" >> /root/devops-demo/Jenkinsfile
