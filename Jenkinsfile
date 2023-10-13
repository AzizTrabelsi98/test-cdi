pipeline{
    agent any



    stages {


        stage('Getting project from Git') {
            steps{
      			checkout([$class: 'GitSCM', branches: [[name: '*/main']],
			extensions: [],
			userRemoteConfigs: [[url: 'https://github.com/AzizTrabelsi98/test-cdi.git']]])
            }
        }


       stage('Cleaning the project') {
            steps{
                	sh "mvn -B -DskipTests clean  "
            }
        }



        stage('Artifact Construction') {
            steps{
                	sh "mvn -B -DskipTests package "
            }
        }



         stage('Unit Tests') {
            steps{
               		 sh "mvn test "
            }
        }



        stage('Code Quality Check via SonarQube') {
            steps{

             		sh "  mvn clean verify sonar:sonar -Dsonar.projectKey=cicd -Dsonar.host.url=http://172.10.0.140:9000/-Dsonar.login=sqp_5b9561fb0e72e73c93bfa5b5f966ec6dba0322b4 "

            }
        }


        stage('Publish to Nexus') {
            steps {


  sh "mvn clean verify sonar:sonar -Dsonar.projectKey=cicd  -Dsonar.projectName='cicd' -Dsonar.host.url=http://172.10.0.140:9000 -Dsonar.token=sqp_5b9561fb0e72e73c93bfa5b5f966ec6dba0322b4"

            }
        }

stage('Build Docker Image') {
                      steps {
                          script {
                            sh 'docker build -t azizoss/spring-app:first .'
                          }
                      }
                  }

                  stage('login dockerhub') {
                                        steps {
                                     // sh 'echo dckr_pat_-SnwrdC_ELsL6it2JT6cgIcAlrs | docker login -u azizbenhaha --password-stdin'
				sh 'docker login -u azizoss --password dckr_pat_9wYLJvVisxnXRtwPxxHrxFZt_Dw'
                                            }
		  }
	    
	                      stage('Push Docker Image') {
                                        steps {
                                   sh 'docker push amirovvv/spring-app:first'
                                            }
		  }


		   stage('Run Spring && MySQL Containers') {
                                steps {
                                    script {
                                      sh 'docker-compose up -d'
                                    }
                                }
                            }

	    



     
}

	    
        post {
		/*success{
		mail bcc: '', body: '''Dear Med Aziz, 
we are happy to inform you that your pipeline build was successful. 
Great work ! 
-Jenkins Team-''', cc: '', from: 'mohamedaziz.benhaha@esprit.tn', replyTo: '', subject: 'Build Finished - Success', to: 'mohamedaziz.benhaha@esprit.tn'
		}
		
		failure{
mail bcc: '', body: '''Dear Med Aziz, 
we are sorry to inform you that your pipeline build failed. 
Keep working ! 
-Jenkins Team-''', cc: '', from: 'mohamedaziz.benhaha@esprit.tn', replyTo: '', subject: 'Build Finished - Failure', to: 'mohamedaziz.benhaha@esprit.tn'
		}*/

       always {
		//emailext attachLog: true, body: '', subject: 'Build finished',from: 'mohamedaziz.benhaha@esprit.tn' , to: 'mohamedaziz.benhaha@esprit.tn'
            cleanWs()
       }
    }

    
	
}
       
