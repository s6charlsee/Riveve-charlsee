pipeline {
    agent any

    stages {
        stage('clean env') {
            steps {
                sh '''
            docker system prune -fa || true
                '''
            }
        }
      

        
      stage('Test maven-cart') {
	     agent {
           docker {
             image 'maven:3.8.7-openjdk-18'
           }
          }
           steps {
               sh '''
           cd $WORKSPACE/REVIVE/src/cart/
           mvn  test  -Dmaven.test.skip=true --quiet
               '''
           }
       }

      stage('Test maven-ui') {
	     agent {
           docker {
             image 'maven:3.8.7-openjdk-18'
           }
          }
           steps {
               sh '''
           cd $WORKSPACE/REVIVE/src/ui/
           mvn  test  -Dmaven.test.skip=true --quiet
               '''
           }
       }
      stage('Test maven-orders') {
	     agent {
           docker {
             image 'maven:3.8.7-openjdk-18'
           }
          }
           steps {
               sh '''
           cd $WORKSPACE/REVIVE/src/orders/
           mvn  test  -Dmaven.test.skip=true --quiet
               '''
           }
       }

      stage('Test maven-node') {
	     agent {
           docker {
             image 'node'
             args '-u 0:0'
             
           }
          }
           steps {
               sh '''
           cd $WORKSPACE/REVIVE/src/checkout/
           npm install

               '''
           }
       }
        stage('SonarQube analysis') {
                agent {
                    docker {
                      image 'sonarsource/sonar-scanner-cli:4.7.0'
                    }
                   }
                   environment {
            CI = 'true'
            scannerHome='/opt/sonar-scanner'
        }
                steps{
                    withSonarQubeEnv('sonar') {
                        sh "${scannerHome}/bin/sonar-scanner"
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
        stage('build artifact microservice catalog') {
	       agent {
            docker {
              image 'golang:1.20.1'
              args '-u 0:0'
            }
           }
            steps {
                sh '''
            cd $WORKSPACE/REVIVE/src/catalog/
            go build   -buildvcs=false
                '''
              }
  
        }
        stage('build artifact maven-ui') {
	       agent {
           docker {
             image 'maven:3.8.7-openjdk-18'
           }
          }
           steps {
               sh '''
           cd $WORKSPACE/REVIVE/src/ui/
           mvn  package -Dmaven.test.skip=true --quiet
               '''
           }
       }
       stage('build artifact maven-orders') {
	     agent {
           docker {
             image 'maven:3.8.7-openjdk-18'
           }
          }
           steps {
               sh '''
           cd $WORKSPACE/REVIVE/src/orders/
           mvn  package -Dmaven.test.skip=true --quiet
               '''
           }
       }
       stage('build artifact maven-node') {
	     agent {
           docker {
             image 'node'
             args '-u 0:0'
             
           }
          }
           steps {
               sh '''
           cd $WORKSPACE/REVIVE/src/checkout/
           npm install
           npm run build
               '''
           }
       }
       
        
        
    }

     
  // post {
  //    success {
  //        slackSend color: '#2EB67D',
  //        channel: 'charlsee-revive', 
  //        message: "REVIVE Project Build Status" +
  //        "\n Project Name: REVIVE" +
  //        "\n Job Name: ${env.JOB_NAME}" +
  //        "\n Build number: ${currentBuild.displayName}" +
  //        "\n Build Status : SUCCESS" +
  //        "\n Build url : ${env.BUILD_URL}"
  //    }
  //    failure {
  //        slackSend color: '#E01E5A',
  //        channel: 'charlsee-revive',  
  //        message: "revive Project Build Status" +
  //        "\n Project Name: REVIVE" +
  //        "\n Job Name: ${env.JOB_NAME}" +
  //        "\n Build number: ${currentBuild.displayName}" +
  //        "\n Build Status : FAILED" +
  //        "\n Build User : Tia" +
  //        "\n Action : Please check the console output to fix this job IMMEDIATELY" +
  //        "\n Build url : ${env.BUILD_URL}"
  //    }
  //    unstable {
  //        slackSend color: '#ECB22E',
  //        channel: 'charlsee-revive', 
  //        message: "revive Project Build Status" +
  //        "\n Project Name: REVIVE" +
  //        "\n Job Name: ${env.JOB_NAME}" +
  //        "\n Build number: ${currentBuild.displayName}" +
  //        "\n Build Status : UNSTABLE" +
  //        "\n Action : Please check the console output to fix this job IMMEDIATELY" +
  //        "\n Build url : ${env.BUILD_URL}"
  //    }   
  //}
}    