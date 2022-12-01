node {

    stage("Git Clone"){

        git credentialsId: 'GIT_CREDENTIALS', url: 'https://github.com/Giridharab/springboot-with-docker.git'
    }

     stage('Gradle Build') {

       sh './gradlew build'

    }

    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t jhooq-docker-demo .'
        sh 'docker image list'
        sh 'docker tag jhooq-docker-demo giridhar7/jhooq-docker-demo:jhooq-docker-demo'
    }

    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
        sh 'docker login -u giridhar7 -p $PASSWORD'
    }

    stage("Push Image to Docker Hub"){
        sh 'docker push  giridhar7/jhooq-docker-demo:jhooq-docker-demo'
    }

    stage("SSH Into k8s Server") {
            steps{
                sshagent(credentials:['SSH_CREDENTIALS']){
    
                stage('Sudoing onto k8smaster') {
                    sh "sudo -i"
                    echo "Accessed as sudo user"
                }
    
    
                stage('Put k8s-spring-boot-deployment.yml onto k8smaster') {
                    sshPut remote: remote, from: 'k8s-spring-boot-deployment.yml', into: '.'
                }
    
                stage('Deploy spring boot') {
                  sshCommand remote: remote, command: "kubectl apply -f k8s-spring-boot-deployment.yml"
                }
            }
        }
    }

}
