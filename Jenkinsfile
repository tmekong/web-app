pipeline {
    agent any
    triggers {
        cron('H */1 * * * ')
     }
    tools {
        maven 'M2_HOME'
      
    }
    stages {
      stage('Build'){
        steps {
          echo "Build step"
          sh 'mvn clean'
          sh 'mvn install'
          sh 'mvn package'
        }
      
      
      }
      stage('sonaqube analys'){
         steps{
         withSonarQubeEnv('sonar') {
                 sh 'mvn sonar:sonar'
              }
         }
      }
       stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
        stage('test '){
        steps {
            retry(4){
          echo  "test step"
            }
          sh 'mvn test'
        }
      
      
      }
        stage('deploy'){
        steps {
            
         sshPublisher(publishers: [sshPublisherDesc(configName: 'Docker-host', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker rmi pcontact:v.${BUILD_NUMBER}; docker build -t pcontact:v.${BUILD_NUMBER} .; docker tag pcontact:v.${BUILD_NUMBER} kserge2001/pcontact:v.${BUILD_NUMBER}; docker push kserge2001/pcontact:v.${BUILD_NUMBER};', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)]) 
           
        }
      
      
      }
    
    }
    post {
        always {
            echo "Always display this message "
        }
        failure {
            echo "Job failed "
        }
        success {
            echo "Successful run "
        }
        unstable {
            echo "The job is unstable "
        }
    } 

}
echo "Jenkinsfile"
