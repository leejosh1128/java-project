properties([pipelineTriggers([githubPush()])])

node('linux') {   
	stage('Unit Test') {
		git 'https://github.com/leejosh1128/java-project.git'
		sh 'ant -f test.xml -v'
		junit 'reports/result.xml'
	}   
	stage('Build') {    
		sh 'ant -f build.xml -v'
	}
	stage('Deploy') {
		sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://as10buckettesting'
	}

	stage('Report') {
		withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '9c02272a-44c3-4440-a976-6be6ab49c54b', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
		sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
}
	}
}
