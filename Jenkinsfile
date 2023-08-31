def buildNumber = currentBuild.number
def startTimeMiliSecond = currentBuild.startTimeInMillis
def startTime = new Date(startTimeMiliSecond)
// def saat = startTime.format("HH:mm:ss")
def zipFileName = "examples_${buildNumber}.zip"
def filePath = '/var/lib/jenkins/workspace/task-jenkins'
pipeline {
    agent any
        stages {
            stage('Github Repo Clone') {
                // environment {
                //     cloneFolderName = 'hermit'
                // }
                // steps {
                //     sh 'rm -rf $cloneFolderName'
                //     sh 'git clone https://github.com/facebookexperimental/hermit'
                // }
                 cleanWs()
                    checkout scm: [ $class: "GitSCM",
                    userRemoteConfigs: [[url: "https://github.com/facebookexperimental/hermit",
                    credentialsId: jenkins-user]],
                    branches: [[name: "main"]]
                    ]
            }
            stage('Github Repo ZIP') {
                steps {
                    script {
                        dir("${filePath}/hermit/") {
                        sh "zip -r ${zipFileName} examples/"
                        sh "cp -R ${zipFileName} ../buckets/${zipFileName}"
                        }
                    }
                }
            }

            stage('Upload Aws S3') {
                steps {
                script {
                        def s3Bucket = 'myjnknsbckt'
                        def awsCliPath = '/usr/local/bin/aws'
                        withCredentials([[
                            $class: 'AmazonWebServicesCredentialsBinding',
                            accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                            secretKeyVariable: 'AWS_SECRET_ACCESS_KEY',
                            credentialsId: 'jenkins-user'
                        ]]) {
                            sh """
                            ${awsCliPath} s3 cp /var/lib/jenkins/workspace/task-jenkins/buckets/examples_${buildNumber}.zip s3://${s3Bucket}/
                            """
                        }
                }
                }
            }
        }
}
