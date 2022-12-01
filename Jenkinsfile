node {

    stage("Git Clone"){

        git credentialsId: 'GIT_CREDENTIALS', url: 'https://github.com/Giridharab/springboot-with-docker.git'
    }

     stage('Gradle Build') {

       sh './gradlew build'

    }

    stage("Docker build"){
        sh 'sudo docker version'
        sh 'sudo docker build -t jhooq-docker-demo .'
        sh 'sudo docker image list'
        sh 'sudo docker tag jhooq-docker-demo giridhar7/jhooq-docker-demo:jhooq-docker-demo'
    }

    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
        sh 'sudo docker login -u giridhar7 -p $PASSWORD'
    }

    stage("Push Image to Docker Hub"){
        sh 'sudo docker push  giridhar7/jhooq-docker-demo:jhooq-docker-demo'
    }

    stage("SSH Into k8s Server") {
        steps{
            sshagent(credentials:['ssh_credentials']){

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
