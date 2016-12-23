node ('maven') {


        stage "Build"
                git url: "https://github.com/rhpoc/openshift-quickstarts"

                sh "cd todolist/todolist-jdbc; mvn clean package -DskipTests=true"
                stash name: "todolist", includes: "todolist/*"

        stage 'Test and Analysis'

                     unstash "todolist"
                     sh "cd todolist/todolist-jdbc; mvn test"
                     //step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])


                     //sh "mvn jacoco:report sonar:sonar -Dsonar.host.url=http://http://sonarqube-openshift-docker-openshift-ticket-monster-dev.apps.rhpoc.com -DskipTests=true"
                     //withSonarQubeEnv('SonarQubeServer') {
                         //sh "cd todolist/todolist-jdbc; mvn sonar:sonar -Dsonar.host.url=http://sonarqube-openshift-docker-openshift-ticket-monster-dev.apps.rhpoc.com -DskipTests=true"
                         //sh "cd todolist/todolist-jdbc; mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar -DskipTests=true"
                         sh "cd todolist/todolist-jdbc; mvn sonar:sonar -Dsonar.host.url=http://sonarqube-openshift-docker-openshift-ticket-monster-dev.apps.rhpoc.com/ -DskipTests=true"

                     //}
                /*
                stage('SonarQube analysis') {
                     withSonarQubeEnv('My SonarQube Server') {
                      // requires SonarQube Scanner for Maven 3.2+
                      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
                     }
                }
                */


}

