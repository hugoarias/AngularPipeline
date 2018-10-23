node {
   checkout scm

   stage('Get dependencies') {
      bat 'npm install'
   }
   stage('Build') {
      bat 'node_modules/.bin/ng build'
   }
   stage('Unit Tests') {
      bat 'node_modules/.bin/ng test --browsers ChromeHeadless --watch=false --code-coverage=true'
   }
   stage('Publish to Dev Env') {
      archive 'dist/AngularPipeline/*.*'

      // Copy to dev env
	  bat 'aws s3 cp ./aws/cloudformation/templates s3://incentral-deploy-artifacts/ --recursive'

	  updateCloudformation('aws cloudformation update-stack --stack-name incentral --template-url https://s3-us-west-2.amazonaws.com/incentral-deploy-artifacts/s3.json --parameters ParameterKey=BucketName,ParameterValue=incentral --no-use-previous-template')
	  updateCloudformation('aws cloudformation update-stack --stack-name cloudfront --template-url https://s3-us-west-2.amazonaws.com/incentral-deploy-artifacts/cloudfront.json --parameters ParameterKey=BucketName,ParameterValue=incentral --no-use-previous-template')

	  copyFileToS3('aws s3 cp ./dist/AngularPipeline/ s3://incentral --recursive --acl public-read-write --exclude "./dist/AngularPipeline/runtime.js.map"');
   }
   stage('E2E Tests') {
     // Replace with actual commands for e2e tests
      parallel testChrome: {
        bat(/node_modules\.bin\ng test --browsers ChromeHeadless --watch=false/)
      }, testFirefox: {
        bat(/node_modules\.bin\ng test --browsers ChromeHeadless --watch=false/)
      }, testIE: {
        bat(/node_modules\.bin\ng test --browsers ChromeHeadless --watch=false/)
      }
   }
   stage('Results') {
      step([$class: 'JUnitResultArchiver', testResults: 'src/HeadlessChrome_0.0.0_(Windows_10_0.0.0)/*.xml'])

      publishHTML([allowMissing: true, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'coverage/', reportFiles: 'index.html', reportName: 'Code Coverage', reportTitles: ''])
   }
   stage('Public files') {
     input 'Do you want to make files public?'
	 bat 'aws cloudfront create-invalidation --distribution-id EBRDWH0SBYNCP --paths /*'

     // Copy files to staging
     // Run e2e tests and so on...
   }
}

def updateCloudformation(url) {
	try {
		bat url
	} catch(e) {
		println(e);
	}
}

def copyFileToS3(commnad) {
	try {
		bat command
	} catch (e) {
		println(e)
	}
}
