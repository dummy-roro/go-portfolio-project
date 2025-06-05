pipeline {
    agent any 

    environment {
        REGISTRY = 'docker.io'
        DOCKER_USERNAME = credentials('DOCKER_USERNAME')
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
        REPO_PAT = credentials('REPO_PAT')
    }

    options {
        skipDefaultCheckout(true)
    }

    stages {
      stage('Cleaning Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from Git') {
            steps {
                git credentialsId: 'GITHUB', url: 'https://github.com/dummy-roro/go-portfolio-project.git'
            }
        }

        stage('Setup Go') {
            steps {
                sh '''
                wget https://go.dev/dl/go1.22.linux-amd64.tar.gz
                sudo rm -rf /usr/local/go
                sudo tar -C /usr/local -xzf go1.22.linux-amd64.tar.gz
                export PATH=$PATH:/usr/local/go/bin
                go version
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'go build -o go-app'
            }
        }

        stage('Test') {
            steps {
                sh 'go test ./...'
            }
        }

        stage('Code Quality') {
            steps {
                sh '''
                curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s v1.56.2
                ./bin/golangci-lint run
                '''
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    def tag = "${env.BUILD_NUMBER}"
                    sh """
                    docker build -t $DOCKER_USERNAME/go-portfolio:${tag} .
                    docker push $DOCKER_USERNAME/go-portfolio:${tag}
                    """
                }
            }
        }

        stage('Image Scanning') {
            steps {
                sh '''
                docker scout cves --only-severities critical,high $DOCKER_USERNAME/go-portfolio:${BUILD_NUMBER}
                '''
            }
        }

        stage('Update Helm Tag') {
            steps {
                dir('go-portfolio-project-manifests') {
                    sh '''
                    git clone -b master https://$REPO_PAT@github.com/dummy-roro/go-portfolio-project-manifests.git .
                    sed -i 's/tag: .*/tag: "'${BUILD_NUMBER}'"/' values.yaml
                    git config user.email "dummy-roro@gmail.com"
                    git config user.name "dummy-roro"
                    git add values.yaml
                    git commit -m "Updated values.yaml file"
                    git push origin master
                    '''
                }
            }
        }
    }
}
