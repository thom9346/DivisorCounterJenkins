pipeline {
    agent any

    triggers {
        pollSCM("* * * * *")
    }
    stages {
        stage("Build") {
            steps {
                bat "docker-compose build"
            }
        }
        stage("Deliver") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    bat 'docker login -u %USERNAME% -p %PASSWORD%'
                    bat "docker-compose push"
                }
            }
        }
        stage("Deploy to Docker Swarm") {
            steps {
                bat "docker service update divisor-counter-service --image %USERNAME%/divisorcounter-counterservice:$BUILD_NUMBER || docker service create --name divisor-counter-service --replicas 3 --publish 80:80 %USERNAME%/divisorcounter-counterservice:$BUILD_NUMBER"
                bat "docker service update cache-service --image %USERNAME%/divisorcounter-cacheservice:$BUILD_NUMBER || docker service create --name cache-service --replicas 3 --publish 5007:80 %USERNAME%/divisorcounter-cacheservice:$BUILD_NUMBER"
            }
        }

    }
}
