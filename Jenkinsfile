  pipeline {
    environment {
      registry = "pratush43/dock"
      registryCredential = 'dockerhub'
      image = '' 
    }
    agent none
      stages {
        
          stage('Build') {
            agent {
      node{
      label 'micro'
      } 
    }
              steps {
             script{ env.buildname = input  message: 'Please input build name', parameters: [string(defaultValue:'', description: 'Enter a valid build name?', name: 'build name')], ok : 'Build Now',id :'choice_id'
             }
                buildName env.buildname
                  sh 'dotnet build'
                archiveArtifacts artifacts: 'bin/Debug/net6.0/*.dll'
                stash includes: 'bin/Debug/net6.0/*.dll', name: 'build', useDefaultExcludes: false
              }
          }
          stage("docker image"){
            agent any
        steps{
          script {
            unstash 'build'
          def dockerImage = docker.build registry + ":$BUILD_NUMBER" + "-$env.buildname"
            docker.withRegistry( '', registryCredential ) {
              dockerImage.push()
          }
        }

      
    }
      }
     
      }
      }
