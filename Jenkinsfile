pipeline {
    agent {
        kubernetes {
            yamlFile 'build-agent.yaml'
        }
    }
    stages {
        stage('Package') {
            parallel {
                stage('Create Jarfile') {
                    steps {
                        container('maven') {
                            sh 'mvn package -DskipTests'
                        }
                    }
                }
                stage('OCI Image BnP') {
                    steps {
                        container('kaniko') {
                            // Replace xxxxxx with your Docker Hub ID
                            sh '''
                            /kaniko/executor -f `pwd`/Dockerfile \
                            -c `pwd` \
                            --insecure \
                            --skip-tls-verify \
                            --cache=true \
                            --destination=docker.io/shende12rahul2/dso-demo:latest
                            '''
                        }
                    }
                }
            }
        }
    }
}
