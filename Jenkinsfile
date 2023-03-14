pipeline{
    agent{ label ('jfrog_1') }
    stages{
         stage('vcs'){
            steps{
               git branch: 'develop', url: 'https://github.com/Sharavani2/spring-petclinic.git'  
            }
         }
         stage('build'){
            steps{
                sh '/opt/maven/bin/mvn package' 
            }
         }
         stage('archiving artifacts'){
            steps{
                 archiveArtifacts artifacts : '**/spring-petclinic-3.0.0-SNAPSHOT.jar'
                                  junit '**/*.xml'
            }
         }
          stage('craeting folder'){
            steps{
                sh "mkdir -p /tmp/${JOB_NAME}/${BUILD_ID}"
                sh "cp -r **/spring-petclinic-*.jar /tmp/${JOB_NAME}/${BUILD_ID}"
                sh "aws s3 sync /tmp/${JOB_NAME}/${BUILD_ID} s3://jenkins789 --acl public-read-write"
            }
          }
      agent{ label ( 'ansible_node1' )}
      stages{
        stage('deployment'){
          steps{
            sh 'ansible-playbook -i hosts spc-project.yaml'
          }
        }
      }

    }

}
    
    
    
  
    
   