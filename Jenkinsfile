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

        stage ('Build_Docker'){
            steps {
                script {
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t abhishekf5/cicd-e2e:${BUILD_NUMBER} .
                    '''
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
                    sh 'cd java-maven-sonar-argocd-helm-k8s/spring-boot-app && docker build -t ${DOCKER_IMAGE} .'
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                    dockerImage.push()
                }
            }
        }
    }
        
    
    
}