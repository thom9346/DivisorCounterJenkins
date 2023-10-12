pipeline{
    agent any

    triggers {
        pollSCM("* * * * *")
    }
    stages {
        stage("FirstStageTest") {
            steps {
                echo "we are running.."
            }
        }
    }
}