def GTHB = 'hermit'
pipeline {
    agent any
    stages {
        stage('Github Repo Clone') {
            steps {
                
                sh 'echo $GTHB'
                sh 'rm -rf $GTHB'
                sh 'git clone https://github.com/facebookexperimental/hermit'
            }
        }
    }
}