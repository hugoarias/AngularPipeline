node {
   checkout scm

   stage('Get dependencies') {
       bat(/npm install/)
   }
   stage('Build') {
      bat(/node_modules\.bin\ng build/)
   }
   stage('Unit tests') {
      bat(/node_modules\.bin\ng test --browsers ChromeHeadless --watch=false --code-coverage=true/)
   }
   stage('Publish to devenv') {
     archive 'dist/AngularPipeline/*.*'
     // Copy to dev env
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
   stage('Staging') {
     input 'Copy Files to staging?'
     // Copy files to staging
     // Run e2e tests and so on...
   }
}
