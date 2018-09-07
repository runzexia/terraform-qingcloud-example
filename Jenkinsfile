pipeline {
  agent any
  triggers {
      cron('H/4 * * * 1-5')
      pollSCM('0 0 * * 0')
  }
  stages {
    stage('lalala') {
      parallel {
        stage('test1') {
          steps {
            echo 'hhhh'
            echo 'hhhh'
          }
        }
        stage('1') {
          steps {
            archiveArtifacts(fingerprint: true, allowEmptyArchive: true, artifacts: 'README.md', caseSensitive: true, defaultExcludes: true)
          }
        }
        stage('error') {
          steps {
            sh 'sh `ls -a`'
          }
        }
        stage('add images readme') {
          steps {
            sh 'sh `echo "hhh" > images/README.md`'
          }
        }
      }
    }
    stage('error') {
      steps {
        archiveArtifacts(allowEmptyArchive: true, caseSensitive: true, defaultExcludes: true, fingerprint: true, artifacts: 'images/*')
      }
    }
    stage('3') {
      steps {
        archiveArtifacts(artifacts: 'README.md', allowEmptyArchive: true, defaultExcludes: true, caseSensitive: true, fingerprint: true)
      }
    }
  }
  environment {
    aaa = 'aaaa'
  }
}
