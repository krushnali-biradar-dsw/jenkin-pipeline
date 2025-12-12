pipeline {
    agent {
        kubernetes {
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: kaniko
                image: gcr.io/kaniko-project/executor:latest
                command:
                - sleep
                args:
                - 9999999
                volumeMounts:
                - name: dockerfile-storage
                  mountPath: /workspace
              volumes:
              - name: dockerfile-storage
                emptyDir: {}
            '''
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                container('kaniko') {
                    sh '/kaniko/executor --dockerfile=Dockerfile --context=. --destination=simple-node-app:latest --no-push'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                // Note: Kaniko builds but doesn't run containers. For local testing, run manually or use a separate agent.
                // Example: sh 'docker run -d -p 3000:3000 simple-node-app:latest'
                echo 'Image built. Run manually: docker run -d -p 3000:3000 simple-node-app:latest'
            }
        }
    }
}