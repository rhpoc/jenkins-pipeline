node {
	stage "Setup"
		echo "Hello there";
		git url: "https://github.com/rhpoc/ticket-monster"

	
	stage "Build"
		sh "cd ticket-monster/demo"
		sh "mvn clean package -Ppostgresql-openshift"

}
