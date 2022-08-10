pipeline {

	agent any

	environment {
	DOCKERHUB_CREDENTIALS_PSW = credentials('Najwa-dockerhub-token')
        DOCKERHUB_CREDENTIALS_USR = 'Najwa'
	AWS_ACCESS_KEY_ID     = credentials('NajwaN-aws-secret-key-id')
  	AWS_SECRET_ACCESS_KEY = credentials('NajwaN-aws-secret-access-key')
	ARTIFACT_NAME = 'Dockerrun.aws.json'
	AWS_S3_BUCKET = 'najwa-belt2-artifacts-123456'
	//AWS_EB_APP_NAME = 'Najwa_dockerhub'
        AWS_EB_ENVIRONMENT_NAME = 'Najwadockerhub-env'
        AWS_EB_APP_VERSION = "${BUILD_ID}"
        AWS_REGION = 'us-east-1'
	}

	stages {

		stage('Build') {
			steps {
				sh 'docker build -t najwadv96/runaway:latest .'
			}
		}

		stage('Login') {
			steps {
				sh 'docker login -u=$DOCKERHUB_CREDENTIALS_USR -p=$DOCKERHUB_CREDENTIALS_PSW'
			}
		}

		stage('Push') {
			steps {
				sh 'docker push mohanadsinan/runaway:latest'
			}
		}

        stage('Deploy') {
            steps {
                sh 'aws configure set region $AWS_REGION'
                sh 'aws elasticbeanstalk create-application-version --application-name Najwa_dockerhub --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'
                sh 'aws elasticbeanstalk update-environment --application-name Najwa_dockerhub --environment-name $AWS_EB_ENVIRONMENT_NAME --version-label $AWS_EB_APP_VERSION'
            }
	}
    }
	post {
		always {
			sh 'docker logout'
		}
	}
}
