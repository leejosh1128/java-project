properties([pipelineTriggers([githubPush()])])

node('linux') {   
	stage('UnitTest') {
		sh 'ant'
		sh 'ant -f test.xml -v'
		junit 'reports/result.xml'
	}   
	stage('Build') {    
		sh 'ant'
		sh 'ant -f build.xml -v'
	}
	stage('Deploy') {
		sh 'aws s3 cp /workspace/java-pipeline/dist/ s3://bucket/elasticbeanstalk-us-east-1-651903360651 --recursive --exclude "*" --include "*.jar"'
	}

	stage('Report') {    
		junit 'reports/*.xml'
	}
}
