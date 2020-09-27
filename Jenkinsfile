node {
    // Mark the code checkout 'stage'....
    stage 'Checkout'
    // Get some code from a GitHub repository
    checkout scm
    
    docker.image('maven:3.3.9-jdk-7').inside {
        stage 'Compile'
        sh "mvn compile"
        stage 'Test'
        sauce('saucelabs') {
            sauceconnect(useGeneratedTunnelIdentifier: true, verboseLogging: true) {
                sh "mvn test"
            }
        }
		 stage('Push Image') {
            steps {
                script {
			        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
			        	app.push("${BUILD_NUMBER}")
			            app.push("latest")
			        }
                }
            }
    }

    stage 'Collect Results'
    step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
    step([$class: 'SauceOnDemandTestPublisher'])
}