import groovy.sql.Sql


@Field 
def output = ""

pipeline {
    agent any
    stages {
       
		stage('Create tabless'){
			 steps{
                script{
		 	sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM staff)=100 THEN 1 ELSE 0 END as output") { row ->
				output= "$row.output"
		}
			writeFile(file: 'output.txt', text: output)
                  	
      }
}}}
	post {
                always {
                    archiveArtifacts artifacts: 'output.txt', allowEmptyArchive: true
                }
            }
}

@NonCPS
    def sqlconnection() {
        String URL = "jdbc:teradata://wbtlabserver.teradata.com/KH255051";
        String username = "KH255051";
        String password = "bSji5jkv";
        String driver= "com.teradata.jdbc.TeraDriver"
        
        echo "Building connection"
        def sqlconn = Sql.newInstance(URL, username, password, driver)
        return sqlconn
    }
