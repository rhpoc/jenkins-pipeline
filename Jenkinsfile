node ('maven') {


        stage "Build"

                git url: "https://github.com/rhpoc/openshift-quickstarts"
                sh "cd todolist/todolist-jdbc; mvn clean package -DskipTests=true"
                stash name: "todolist", includes: "todolist/*"

        stage 'Test and Analysis'

		unstash "todolist"
		sh "cd todolist/todolist-jdbc; mvn test"
		sh "cd todolist/todolist-jdbc; mvn sonar:sonar -Dsonar.host.url=http://sonarqube-openshift-docker-openshift-ticket-monster-dev.apps.rhpoc.com/ -DskipTests=true"
	
	stage 'Push to Nexus'

		
	stage 'Deploy'

            sh "cd todolist/todolist-jdbc; mvn -s settings.xml deploy:deploy-file -DgroupId=todolist -DartifactId=todolist-jdbc -Dversion=1.0.0-SNAPSHOT -DgeneratePom=true -Dpackaging=war -DrepositoryId=nexus -Durl=http://nexus-openshift-docker-openshift-ticket-monster-dev.apps.rhpoc.com/content/repositories/snapshots/ -Dfile=target/todolist-jdbc.war";


			

}

