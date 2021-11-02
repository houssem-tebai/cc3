def commit_id
pipeline {
    agent any
    stages {
        stage('preparation') {
            steps {
                checkout scm
                sh "git rev-parse --short HEAD > .git/commit-id"
                script {
                    commit_id = readFile('.git/commit-id').trim()
                }
            }
        }
        stage ('code quality') {
            steps {
                echo 'testing code quality'
                sh "mvn clean verify sonar:sonar\
                   -Dsonar.projectKey=position-simulator\
                    -Dsonar.host.url=http://localhost:9000\
                    -Dsonar.login=c2630b0249f1c5e483810db298fccfe2be330e3b "
                echo 'code test complete'
            }
        }
        stage ('build') {
            steps {
                echo 'building maven workload'
                sh "mvn clean install"
                echo 'build complete'
            }
        }

        stage ("image build") {
            steps {
                echo 'building docker image'
                sh "docker build -t cc3/position-simulator:${commit_id} ."
                echo 'docker image built'
            }
        }
    }
}