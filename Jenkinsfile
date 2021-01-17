import groovy.sql.Sql


output = ""
sqlconn= null

pipeline {
    agent any
	
    
    stages {
	    stage('build sql connection'){
	    	steps{
			script{
				sqlconnection()
			}}
	    }
		stage('Create tabless'){
			 steps{
                script{
		 	sql_connection.eachRow("SELECT CASE WHEN (SELECT count(*) FROM staff)=100 THEN 1 ELSE 0 END as output") { row ->
				output= "All rows inserted"+"\t\t\t$row.output"
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
        sqlconn = Sql.newInstance(URL, username, password, driver)
        return sqlconn
    }
