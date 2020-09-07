#!groovy
pipeline {
    agent {
        label("agent")
    }
    options {
        timestamps()
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
                sh './gradlew clean build publish'                
            }
        }
    }
}
