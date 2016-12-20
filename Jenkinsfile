node ('maven') {
	stage "Setup"
		echo "About to clone git url";
		git branch: "2.7.0.Final-with-tutorials", url: "https://github.com/rhpoc/ticket-monster"

	
	stage "Build"
		echo "pwd: " 
		sh "pwd";

		sh "ls -la demo/";

		echo "About to run mvn command";
		sh "cd demo; mvn clean package -Ppostgresql-openshift"

}
