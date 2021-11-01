def commit_id
pipeline{
    agent any
    stages{
        stage("preparation"){
            steps{
                echo "getting commit id ...."
                sh "git rev-parse --short HEAD > .git/commit_id"
                script {
                    commit_id = readFile('.git/commit_id').trim()
                }
            }
        }
        stage("code build"){
            steps{
                echo "building ....."
                sh "mvn clean install"
                echo "building finished"
            }
        }
        stage("containerisation"){
            steps{
                echo "building docker images ....."
                sh "docker build -t positionsim:$commit_id ."
                echo "building finished"
            }
        }
    }

}