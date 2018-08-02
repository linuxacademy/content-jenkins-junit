pipeline {
  agent none

    options {
     buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
      }    


  stages {
    stage('Unit Tests') {
        agent {
            label 'apache'
        }
        steps {
        sh 'ant -f test.xml -v'
          junit 'reports/result.xml'
        }
      }
      
    stage('Build') {
        steps {
        sh 'ant -f build.xml -v'
        }
       }
      
    stage('Deploy') {
        steps {
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all"
     }   
   }
 }
  
    stage ("Running on CentOS") {
        agent {
            label 'CentOS'
        }
        steps {
            sh "wget http://tpavan-d69ca7ed1.mylabserver.com/rectangles/all/recatangle_${env.BUILD_NUMBER}.jar"
            sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
        }
    }
  post {
    always {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
      }
   }
}