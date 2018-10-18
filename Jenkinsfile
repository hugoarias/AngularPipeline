node {
   checkout scm
   
   //stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      //git 'https://github.com/hugoarias/AngularPipeline.git'
   //}
   stage('Get dependencies') {
       bat(/npm install/)
   }
   stage('Build') {
      bat(/node_modules\.bin\ng build/)
   }
   stage('Test') {
       bat(/node_modules\.bin\ng test --browsers ChromeHeadless --watch=false --code-coverage=true/)
   }
   stage('Results') {
      step([$class: 'JUnitResultArchiver', testResults: 'src/HeadlessChrome_0.0.0_(Windows_10_0.0.0)/*.xml'])

      publishHTML([allowMissing: true, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'coverage/', reportFiles: 'index.html', reportName: 'Code Coverage', reportTitles: ''])

      archive 'dist/AngularPipeline/*.*'
   }
}