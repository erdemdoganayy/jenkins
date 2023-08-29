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
        stage('Github Repo ZIP') {
             environment {
                GTHB = 'hermit'
            }
            steps { 
                script {
                    def buildNumarasi = currentBuild.number
                    def baslangicZamaniMilisaniye = currentBuild.startTimeInMillis
                    def baslangicZamani = new Date(baslangicZamaniMilisaniye)
                    def saat = baslangicZamani.format("HH:mm:ss")
                    def zipDosyaAdi = "examples_${buildNumarasi}.zip"

                    dir('hermit/') {
                     
                    sh "zip -r ${zipDosyaAdi} examples/"
                    echo "Oluşturulan zip dosyasının adı: ${zipDosyaAdi}"
                    sh 'ls -al'   
                    }
                }
            }
        }
       
         stage('Örnek Aşama') {
            steps { 

             script {
                    def sourceFilePath = '/var/lib/jenkins/workspace/task-jenkins/hermit/examples_80.zip'
                    def s3Bucket = 'myjnknsbckt'
                    def awsCliPath = '/usr/local/bin/aws' // AWS CLI yolunu buraya ekleyin
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding', 
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                        credentialsId: 'AWS_ID'
                    ]]) {
                    
                    sh """
                    ${awsCliPath} s3 cp ${sourceFilePath} s3://${s3Bucket}/
                    """
                    }
            }
        }
        
    }
}
}
