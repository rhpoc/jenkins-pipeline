node {
	stage "Setup"
		echo "Hello there";
		git clone https://github.com/rhpoc/ticket-monster

	
	stage "Build"
		cd ticket-monster/demo
		mvn clean package -Ppostgresql-openshift

}
