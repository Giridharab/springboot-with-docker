node {

    stage("Clone repository'"){

        checkout scm
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
           sh 'ssh k8smaster'
            }
        }
    }

}
