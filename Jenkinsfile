pipeline {
   agent any
	stages {
      stage('Git Checkout') {
         steps {
            git 'https://github.com/samridhi97/parking_backend.git'
		}
	}
	stage('Build') {
		steps {
			withSonarQubeEnv('sonar') {
				sh '/opt/maven/bin/mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:RELEASE:sonar -Dmaven.test.skip=true'
			}
		}
	}
	stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
       
	stage ('Deploy') {
		steps {
			sh '/opt/maven/bin/mvn clean deploy -Dmaven.test.skip=true'
		}
	}
	stage ('Release') {
		steps {
			sh 'export JENKINS_NODE_COOKIE=dontkillme ;nohup java -jar $WORKSPACE/target/*.jar &'
		}
	}
}
	

}

