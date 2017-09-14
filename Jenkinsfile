pipeline {
  agent any
  stages {
    stage('CheckOut') {
      steps {
        parallel(
          "CheckOut": {
            git(url: 'git@06922352ca60:root/myproj.git', poll: true, branch: 'master', changelog: true)
            
          },
          "PreCheck1": {
            sh 'whoami'
            writeFile(file: 'file1', text: 'this is file-1', encoding: 'utf-8')
            
          },
          "PreCheck2": {
            sh 'uname -a'
            
          }
        )
      }
    }
    stage('Build') {
      steps {
        parallel(
          "Build": {
            sh 'echo "Building now"'
            
          },
          "Build-PreCheck": {
            sh 'echo "Build PreCheck"'
            
          }
        )
      }
    }
    stage('Test') {
      steps {
        parallel(
          "Test": {
            sh 'echo "Test"'
            
          },
          "Test1": {
            sh 'echo "Test 1"'
            
          },
          "Test2": {
            sh 'echo "Test 2"'
            
          },
          "Test3": {
            sh 'echo "Test 3"'
            
          }
        )
      }
    }
    stage('Package') {
      steps {
        sh 'echo "Packageing now"'
      }
    }
    stage('Harbor') {
      steps {
        echo 'img is stroring'
      }
    }
    stage('Staging') {
      steps {
        echo 'staging now'
      }
    }
    stage('Release') {
      steps {
        echo 'release'
      }
    }
    stage('ReadyGo') {
      steps {
        echo 'ReadyToGo'
        input(message: 'Ready to Go Production?', submitter: 'admin', submitterParameter: 'isReady=true')
        echo 'Done'
      }
    }
  }
}