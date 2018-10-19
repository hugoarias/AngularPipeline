node {
   checkout scm

   stage('Get dependencies') {
       bat(/npm install/)
   }
   stage('Build') {
      bat(/node_modules\.bin\ng build/)
   }
   stage('Test') {
      parallel testChrome: {
        bat(/node_modules\.bin\ng test --browsers ChromeHeadless --watch=false --code-coverage=true/)
      }, testFirefox: {
        bat(/node_modules\.bin\ng test --browsers ChromeHeadless --watch=false --code-coverage=true/)
      }, testIE: {
        bat(/node_modules\.bin\ng test --browsers ChromeHeadless --watch=false --code-coverage=true/)
      }
   }
   stage('Results') {
      step([$class: 'JUnitResultArchiver', testResults: 'src/HeadlessChrome_0.0.0_(Windows_10_0.0.0)/*.xml'])

      publishHTML([allowMissing: true, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'coverage/', reportFiles: 'index.html', reportName: 'Code Coverage', reportTitles: ''])

      archive 'dist/AngularPipeline/*.*'
   }
   stage('Staging') {
     input 'Copy Files to staging?'
     // Copy files to staging
   }
}
