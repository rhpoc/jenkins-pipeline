node ('maven') {
	stage "Setup"
		echo "About to clone git url";
		git branch: "2.7.0.Final-with-tutorials", url: "https://github.com/rhpoc/ticket-monster"

	
	stage "Build"
		sh "pwd";
		sh "ls -la demo/";

		echo "About to run mvn command";
		sh "cd demo; mvn clean package -DskipTests=true -Ppostgresql-openshift"

	stage 'Test and Analysis'
             parallel (
                 'Test': {
                     sh "mvn test"
                     step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
                 },
                 'Static Analysis': {
                     sh "mvn jacoco:report sonar:sonar -Dsonar.host.url=http://http://sonarqube-openshift-docker-openshift-ticket-monster-dev.apps.rhpoc.com -DskipTests=true"
                 }
             )

}
