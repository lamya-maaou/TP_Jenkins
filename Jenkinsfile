pipeline {
  agent any

  options {
    timestamps()
    disableConcurrentBuilds()
  }

  // Déclenchement automatique : vérifie le repo chaque minute
  // (équivalent à l'exercice "déclencher le build chaque minute")
  triggers {
    pollSCM('* * * * *')   // vérifie les changements toutes les minutes
    // Alternative si vous voulez une planification stricte :
    // cron('H/1 * * * *') // déclenche au moins 1 fois par minute
  }

  stages {
    stage('Checkout') {
      steps {
        // Si "Pipeline script from SCM" est utilisé, checkout scm suffit
        checkout scm
      }
    }

    stage('Compile Java') {
      steps {
        script {
          if (isUnix()) {
            sh '''
              rm -rf bin && mkdir -p bin
              javac -d bin *.java
            '''
          } else {
            bat '''
              if exist bin rmdir /S /Q bin
              mkdir bin
              javac -d bin *.java
            '''
          }
        }
      }
    }

    stage('Run HelloWorld') {
      steps {
        script {
          if (isUnix()) {
            sh 'java -cp bin HelloWorld'
          } else {
            bat 'java -cp bin HelloWorld'
          }
        }
      }
    }

    stage('Run Merci') {
      steps {
        script {
          if (isUnix()) {
            sh 'java -cp bin Merci'
          } else {
            bat 'java -cp bin Merci'
          }
        }
      }
    }

    stage('Run DeRien') {
      steps {
        script {
          if (isUnix()) {
            sh 'java -cp bin DeRien'
          } else {
            bat 'java -cp bin DeRien'
          }
        }
      }
    }

    stage('Archive .class') {
      steps {
        script {
          // Archive les .class pour consultation depuis Jenkins
          // (non bloquant s'il n'y en a pas)
          try {
            archiveArtifacts artifacts: 'bin/**/*.class', fingerprint: true
          } catch (e) {
            echo "Rien à archiver"
          }
        }
      }
    }
  }

  post {
    always {
      echo 'Pipeline terminée.'
    }
    success {
      echo '✅ SUCCESS'
    }
    failure {
      echo '❌ FAILURE'
    }
  }
}
