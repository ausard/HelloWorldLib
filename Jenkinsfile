#!groovy
pipeline {
    agent {
        label("agent")
    }
    options {
        timestamps()
    }
    parameters {
      booleanParam(defaultValue: false, description: 'Is publish library build to the nexus?', name: 'isPublish')
    }
    stages {
        stage("Prepare Ws") {
            steps {
                cleanWs()
            }
        }
        stage("Git clone") {
            steps {
                git 'https://github.com/ausard/HelloWorldLib.git'
            }
        }
        stage("increment version lib"){
          steps{
            script{
              if (params.isPublish){
                sh './gradlew incrementVersion'
              }
            }
          }
        }
        stage("Build lib") {
            steps {
              sh './gradlew clean build'
            }
        }
        stage('Publish lib build to the Nexus') {
            steps {
                script{
                  if (params.isPublish){
                    sh './gradlew publish'
                  }
                }
            }
        }
        stage('Commit changes on git'){
          steps{
            script{
              if (params.isPublish) {
                properties = readProperties  file: 'gradle.properties'
                versionString = properties."version"

                echo 'This will sync changes on git'
                sh 'git add gradle.properties'
                sh 'git config --global user.name "ausard"'
                sh 'git config --global user.email "ausard@yandex.ru"'
                sh 'git commit -m "Version lib changed to ' + versionString + '"'
                withCredentials([usernamePassword(credentialsId: 'git', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                  sh 'git push --set-upstream https://$USERNAME:$PASSWORD@github.com/ausard/HelloWorldLib.git'
                  sh 'git tag '+versionString
                  sh 'git push --tags https://$USERNAME:$PASSWORD@github.com/ausard/HelloWorldLib.git'
                }
              }
            }
          }
        }
    }
}
