node {
   def mvnHome = tool 'M3'

   stage('Checkout Code') { 
      git 'https://github.com/maping/java-maven-calculator-web-app.git'
   }
   stage('JUnit Test') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' clean test"
      } else {
         bat(/"${mvnHome}\bin\mvn" clean test/)
      }
   }
   stage('Integration Test') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' integration-test"
      } else {
         bat(/"${mvnHome}\bin\mvn" integration-test/)
      }
   }
 /*
   stage('Performance Test') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' cargo:start verify cargo:stop"
      } else {
         bat(/"${mvnHome}\bin\mvn" cargo:start verify cargo:stop/)
      }
   }
  */
 /* stage('Performance Test') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' verify"
      } else {
         bat(/"${mvnHome}\bin\mvn" verify/)
      }
   }*/
  /* stage('Deploy') {
      timeout(time: 10, unit: 'MINUTES') {
           input message: 'Deploy this web app to production ?'
      }
      echo 'Deploy...'
   }*/
           stage ('Deploy to Octopus') {
            steps {
                withCredentials([string(credentialsId: 'OctopusAPIKey', variable: 'API-L9UYJQIPAHYK9EDRAC8D4EP2A10')]) {
                    sh """
                        ${tool('Octo CLI')}/Octo pack --id="Demo-Project" --format="zip" --version="1.0.0.0" --basePath="/tmp/MyPackage" --outFolder="/tmp"
                        ${tool('Octo CLI')}/Octo push --package target/demo.0.0.1-SNAPSHOT.war --replace-existing --server http://13.75.170.128/ --apiKey ${APIKey}
                        ${tool('Octo CLI')}/Octo create-release --project "Demo-Project" --server http://13.75.170.128/ --apiKey ${APIKey}
                        ${tool('Octo CLI')}/Octo deploy-release --project "Demo-Project" --version latest --deployto SIT --server http://13.75.170.128/ --apiKey ${APIKey}
                    """
                }
            }
           }
}
