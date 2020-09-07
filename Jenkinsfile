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
        stage("Build lib and publish to the nexus") {
            steps {
              script{
                if (isIncrementVersion == true){
                  sh './gradlew incrementVersion'
                }
              }
              sh './gradlew clean build publish'
            }
        }
    }
}
