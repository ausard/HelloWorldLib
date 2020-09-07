#!groovy
pipeline {
    agent {
        label("agent")
    }
    options {
        timestamps()
    }
    parameters {
      booleanParam defaultValue: false, description: 'Increment version library', name: 'isIncrementVersion'
      booleanParam defaultValue: false, description: 'Is publish library build to the nexus?', name: 'isPublish'
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
            sh "echo {$params.isIncrementVersion}"
            script{
              if (params.isIncrementVersion == true){
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
                  // def INPUT_PARAMS = input
                  //   message :"Should you've publish lib?",
                  //   ok : 'Yes',
                  //   parameters :[
                  //     booleanParam(name: 'isPublish', defaultValue: false, description: 'Is publish library build to the nexus?')
                  //   ]
                  sh "echo {$params.isPublish}"
                  if (params.isPublish == true){
                    sh './gradlew publish'
                  }
                }
            }
        }
        stage('Commit changes on git'){
          steps{
            script{
              if (params.isIncrementVersion == true and params.isPublish == true){
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
