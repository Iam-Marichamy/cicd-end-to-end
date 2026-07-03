pipeline {
    
    agent any
    environment {
        IMAGE_TAG = "(BULID_NUMBER)"

    }
    stages {

        stage ('Check_out'){
            steps {
                git credentialsId: 'f87a34a8-0e09-45e7-b9cf-6dc68feac670', 
                url: 'https://github.com/iam-Marichamy/cicd-end-to-end',
                branch: 'main'
            }
        }
        stage ('Build_Docker'){
            steps {
                script {
                    sh '''
                    echo 'Buid Docker Image'
                    docker build --no-cache -t ramesh95/ultimate-cicd:${BUILD_NUMBER} .
                    '''
                }
            }
        }
        stage('Static Code Analysis') {
            environment {
                SONAR_URL = "http://13.50.112.223:9000"
            }
            tools {
                    // This must match the exact name you gave it in the Tools menu
                        sonarRunner 'sonar-scanner-5' 
                    }
            steps {
                withCredentials([string(credentialsId: 'SonarQube', variable: 'SONAR_AUTH_TOKEN')]) {
                sh 'sonar-scanner -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL} -Dsonar.projectKey=my-python-app -Dsonar.sources=.'
                }
            }
        }
        stage ('Push the artifacts') {
            environment {
                DOCKER_IMAGE = "ramesh95/ultimate-cicd:${BUILD_NUMBER}"
                // DOCKERFILE_LOCATION = "java-maven-sonar-argocd-helm-k8s/spring-boot-app/Dockerfile"
                REGISTRY_CREDENTIALS = credentials('docker-cred')
            }
            steps {
                script {
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                    dockerImage.push()
                }
            }
        }
    }
}        
}   
    