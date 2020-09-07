#!groovy
pipeline {
    agent {
        label("agent")
    }
    options {
        timestamps()
    }
    parameters {
      booleanParam defaultValue: true, description: 'Is increment version library?', name: 'isIncrementVersion'
      booleanParam defaultValue: true, description: 'Is publish library\'s build to the nexus?', name: 'isPublish'

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
        }
        stage('Publish lib build to the Nexus') {            
            input {
              message "Should you've publish lib?"
              ok 'Yes'
              submitterParameter 'isPublish'
              parameters {
                booleanParam defaultValue: false, description: 'Is publish library build to the nexus?', name: 'isPublish'
              }
            }
            steps {
              script{
                if (isPublish == true){
                  sh './gradlew publish'
                }
              }
            }
        }
    }
}
