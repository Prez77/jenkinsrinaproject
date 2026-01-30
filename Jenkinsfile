pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pointing to your NEW repo
                git branch: 'main', url: 'https://github.com/Prez77/my-web-app1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t local-web-app1:latest .'
            }
        }

        stage('Deploy to K3s') {
            steps {
                sh '''
                cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web-app1
  template:
    metadata:
      labels:
        app: web-app1
    spec:
      containers:
      - name: nginx
        image: local-web-app1:latest
        imagePullPolicy: Never
---
apiVersion: v1
kind: Service
metadata:
  name: web-service1
spec:
  type: NodePort
  selector:
    app: web-app1
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30081
EOF
                '''
            }
        }
    }
}
