pipeline {
    agent {
        kubernetes {
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: kaniko
                image: gcr.io/kaniko-project/executor:debug
                command:
                - /bin/sh
                args:
                - -c
                - sleep 9999999
                volumeMounts:
                - name: dockerfile-storage
                  mountPath: /workspace
                - name: docker-config
                  mountPath: /kaniko/.docker
              - name: kubectl
                image: bitnami/kubectl:latest
                command:
                - sleep
                args:
                - 9999999
              volumes:
              - name: dockerfile-storage
                emptyDir: {}
              - name: docker-config
                secret:
                  secretName: dockerhub-secret
            '''
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                container('kaniko') {
                    sh '/kaniko/executor --dockerfile=Dockerfile --context=. --destination=docker.io/<your-dockerhub-username>/simple-node-app:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                container('kubectl') {
                    sh '''
                    kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: simple-node-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: simple-node-app
  template:
    metadata:
      labels:
        app: simple-node-app
    spec:
      containers:
      - name: simple-node-app
        image: docker.io/<your-dockerhub-username>/simple-node-app:latest
        ports:
        - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: simple-node-app-service
spec:
  selector:
    app: simple-node-app
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
  type: NodePort
EOF
                    '''
                }
            }
        }
    }
}