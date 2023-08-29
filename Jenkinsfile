def buildNumarasi = currentBuild.number
def baslangicZamaniMilisaniye = currentBuild.startTimeInMillis
def baslangicZamani = new Date(baslangicZamaniMilisaniye)
def saat = baslangicZamani.format("HH:mm:ss")
def zipDosyaAdi = "examples_${buildNumarasi}.zip"
def filePath = '/var/lib/jenkins/workspace/task-jenkins'
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
                    dir("$(filePath)/hermit/") {
                    sh "zip -r ${zipDosyaAdi} examples/"
                    sh "cp -R ${zipDosyaAdi} ../buckets/${zipDosyaAdi}"
                    echo "Oluşturulan zip dosyasının adı: ${zipDosyaAdi}"
                    sh 'ls -al'   
                    }
                }
            }
        }
       
         stage('Upload Aws S3') {
            steps { 

             script {
                    def s3Bucket = 'myjnknsbckt'
                    def awsCliPath = '/usr/local/bin/aws' // AWS CLI yolunu buraya ekleyin
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding', 
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID', 
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                        credentialsId: 'AWS_ID'
                    ]]) {
                    
                    sh """
                    ${awsCliPath} s3 cp /var/lib/jenkins/workspace/task-jenkins/buckets/examples_${buildNumarasi}.zip s3://${s3Bucket}/
                    """
                    }
            }
        }
        
    }
}
}
