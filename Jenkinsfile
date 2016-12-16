node {
	stage "Setup"
		echo "About to clone git url";
		git url: "https://github.com/rhpoc/ticket-monster"

	
	stage "Build"
		echo "About to cd into repository";
		sh "cd ticket-monster/demo"

		echo "About to run mvn command";
		sh "mvn clean package -Ppostgresql-openshift"

}
