pipeline {
    agent any
    stages {
        stage('Github Repo Clone') {
            environment {
                GTHB = 'hermit'
            }
            steps { 
                sh 'rm -rf $GTHB'
                sh 'git clone https://github.com/facebookexperimental/hermit'
            }
        }
    }
}