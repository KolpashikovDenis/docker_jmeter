properties([disableConcurrentBuilds()])

pipeline {
   agent any
   triggers { pollSCM('* * * * *') }
   options {
       buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10' ))
       timestamps()
   }
   stages {
      stage('Preparing files') {
         steps {
            sh 'cd /home/denis/jmeter'
            sh 'rm -rf .git'
            sh 'rm -rf simple'
            sh 'git init && git clone https://github.com/KolpashikovDenis/simple.git'
            sh 'cp simple/* /home/denis/projects/first'
         }
      }
      stage('Build docker image'){
          steps{
             sh 'docker build -t jmeter-image /home/denis/projects/first/. '
          }
      }
      stage('Run test'){
          steps{
              sh 'docker run jmeter-image run_test'
          }
      }
      stage('Clean project'){
          steps{
              sh '/home/denis/projects/first/clear_docker_tmp.sh'
              
          }
      }
   }
}
