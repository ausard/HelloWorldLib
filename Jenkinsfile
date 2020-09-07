#!groovy
pipeline {
    agent {
        label("agent")
    }
    options {
        timestamps()
    }
    parameters {
      booleanParam defaultValue: false, description: 'Is increment version library?', name: 'isIncrementVersion'
      booleanParam defaultValue: false, description: 'Is publish build library to the nexus?', name: 'isPublish'
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
              if (isIncrementVersion == true){
                sh './gradlew incrementVersion'
              }
            }
          }
        }
        stage("Build lib") {
            steps {
              sh './gradlew clean build'
            }
            post{
              success{
                script{
                  if (isPublish == true){
                    sh './gradlew publish'
                  }
                }
              }
            }
        }
    }
}
