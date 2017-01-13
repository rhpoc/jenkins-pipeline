node ('maven') {

        def ocCmd = "oc --token=`cat /var/run/secrets/kubernetes.io/serviceaccount/token` --server=https://openshift.default.svc.cluster.local --certificate-authority=/run/secrets/kubernetes.io/serviceaccount/ca.crt"
        
        stage "Build"
            git url: "https://github.com/rhpoc/openshift-quickstarts"
            sh "cd todolist/todolist-jdbc; mvn clean package -DskipTests=true"
            //stash name: "todolist", includes: "todolist/*"

        stage 'Test and Analysis'
            //unstash "todolist"
            sh "cd todolist/todolist-jdbc; mvn test"
            //sh "cd todolist/todolist-jdbc; export JBOSS_MANAGEMENT_ADDRESS=localhost; mvn -Pit test "

            sh "cd todolist/todolist-jdbc; mvn sonar:sonar -Dsonar.host.url=http://sonarqube-openshift-docker-openshift-ticket-monster-dev.apps.rhpoc.com/ -DskipTests=true"

        stage 'Push to Nexus'
            
            //configFileProvider( [configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
            sh "cd todolist/todolist-jdbc; mvn -s settings.xml deploy:deploy-file -DgroupId=todolist -DartifactId=todolist-jdbc -Dversion=1.0.0-SNAPSHOT -DgeneratePom=true -Dpackaging=war -DrepositoryId=nexus -Durl=http://nexus-openshift-docker-openshift-ticket-monster-dev.apps.rhpoc.com/content/repositories/snapshots/ -Dfile=target/todolist-jdbc.war";

        stage 'Deploy to DEV Env'
            
            // prepare war
            sh "cd todolist/todolist-jdbc; mkdir -p oc-build/deployments"
            sh "cd todolist/todolist-jdbc;ls -la target/;"
            sh "cd todolist/todolist-jdbc; cp target/todolist-jdbc.war oc-build/deployments/ROOT.war"
            sh "cd todolist/todolist-jdbc;ls -la oc-build/;"
            
            // remove resources (but leave the image stream)
            sh "${ocCmd}  delete bc,dc,svc,route -l app=todolist -n openshift-ticket-monster-env-dev"
            //sh "${ocCmd} delete is -l app=todolist -n openshift-ticket-monster-env-dev"
            
            // create build
            //sh "${ocCmd} new-build --name=todolist --image-stream=jboss-eap70-openshift --binary=true --labels=app=todolist -n openshift-ticket-monster-env-dev || true"
            sh "${ocCmd} new-build --name=todolist jboss-eap70-openshift~https://github.com/rhpoc/openshift-quickstarts --context-dir=todolist/todolist-jdbc --labels=app=todolist -n openshift-ticket-monster-env-dev || true"
    
            // start build
            sh "ls -la todolist/todolist-jdbc;"
            sh "ls -la todolist/todolist-jdbc/oc-build/deployments"
            //sh "${ocCmd} start-build todolist --from-dir=todolist/todolist-jdbc/oc-build --wait=true -n openshift-ticket-monster-env-dev"

            // deploy image
            sh "${ocCmd}  new-app todolist:latest -e MYDB=java:jboss/jdbc/TodoListDS -e OPENSHIFT_KUBE_PING_NAMESPACE=openshift-ticket-monster-env-dev -n openshift-ticket-monster-env-dev"
            sh "${ocCmd}  expose svc/todolist -n openshift-ticket-monster-env-dev"
            
            sh "cd todolist/todolist-jdbc; mvn test"
            
            // integration test fails because the management port is hard coded (in openshift-launch.sh) to 127.0.0.1
            // The only way to override it would be to extend the S2I image for jboss-eap70-openshift
            //sh "cd todolist/todolist-jdbc; mvn -Pit test "

    
}

