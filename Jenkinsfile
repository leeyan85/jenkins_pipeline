pipeline {
  agent any
  stages {
    stage('CheckOut') {
      steps {
        parallel(
          "CheckOut": {
            git(url: 'git@106.2.4.82:root/myproj.git', poll: true, branch: 'master', changelog: true)
            
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
          "Sonar-Check": {
            withSonarQubeEnv('Sonar') {
                sh 'echo "Build PreCheck"'
                sh 'echo sonarqube-scanner-maven'
                sh 'cd sonarqube-scanner-maven && /opt/apache-maven-3.5.0/bin/mvn clean install sonar:sonar'
            }
          },
          "Build": {
            sh 'echo "Building now"'
            
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
          "JUnitResultArchiver": {
            sh 'echo "Junit Test"'
            junit allowEmptyResults: true, testResults: 'sonarqube-scanner-maven/app-*/target/*-reports/*.xml'
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
        script {
                    env.RELEASE_SCOPE = input message: 'http://106.2.4.82:8080/userContent/releasenotes.html 蓝绿部署验证通过了么？', ok: 'Release!',
                            parameters: [choice(name: 'RELEASE_SCOPE', choices: 'patch\nminor\nmajor', description: 'What is the release scope?')]
                }
        echo "${env.RELEASE_SCOPE}"
        sh "sed -i -e \"s/RELEASE_TYPE/${env.RELEASE_SCOPE} release /\" releasenotes.html " 
        sh 'sed -i -e "s/RELEASE_VERSION/`date "+%Y%M%m%d%H%M"`/" releasenotes.html '
        sh 'cp -f releasenotes.html ~/userContent/releasenotes.html'

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
        script {
                    env.RELEASE_SCOPE = input message: 'User input required', ok: 'Release!',
                            parameters: [choice(name: 'RELEASE_SCOPE', choices: 'patch\nminor\nmajor', description: 'What is the release scope?')]
                }
        echo "${env.RELEASE_SCOPE}"

      }
    }
  }
}
