pipeline {
    agent any
   // tools { // <1>
       // maven 'maven 3.2.5' // <2>
        //jdk 'jdk1.8.0_151' // <3>
   // }
    stages {
        stage ('Initialize') {
            steps {
                println("Initialize")
				bat '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
               println("Build")
			   bat "mvn clean"
            }
        }

        stage ('test') {
                    steps {
					   bat "mvn dependency::tree"
					   bat "curl -1 -k --user badrish:Ruchi@1990 --silent "http://localhost:8080/job/Selenium_Pipeline_CRM_Project/job/master/${BUILD_NUMBER}/consoleText" >console_log.txt

                       bat "grep --text -A 37 -i -r "<<< FAILURE!" console_log.txt >Fail_Tests.txt"

                       bat "mvn clean -U"
					   bat "url=https://badrish.atlassian.net/rest/api/2/issue/TEST-25/comment"
                       bat "curl -D- -k -u badrish.kumar.mishra@gmail.com:mishra123 -X POST --data '{ "body": "update from jenkins: Test from Jenkins"}' -H 'Content-Type: application/json' ${url}"
                    }
        }

    }
}