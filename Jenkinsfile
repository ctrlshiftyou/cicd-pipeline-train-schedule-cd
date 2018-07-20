pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
    
stage('DeployToStaging') {
  when {
      branch 'master'
	  }
	  steps {
	    withCredentilas([usernamePassword(credentialsId: 'webserver_login', uernameVariable: 'USERNAME', passwordVariable
		  sshPublisher{
		     failOnError: true,
			 continueOnError: false,
			 publishers: [
			    sshPublisherDesc{
				   configName: 'staging',
				   SSHCredentials: [
				      username: "USERNAME",
					  encryptedPassphrase: "$USERPASS"
					  ],
					  transfers: [
					    sshTransfer(
						   sourceFiles: 'dist/trainSchedule.zip',
						   removePrefix: 'dist/'
						   remoteDirectory: '/tmp',
						   execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule,
						   )
						  ]
						 )
						]
					)
				}
			}
		}
