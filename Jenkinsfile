import groovy.sql.Sql


output = ""

pipeline {
    agent any
    stages {
       
		stage('Create tabless'){
			 steps{
				 
                script{
		
		 	sqlconnection().eachRow("SELECT COUNT(*) FROM dbc.TABLES WHERE TABLENAME = 'jobs_final' and databasename='KH255051' as output") { row ->
				println($row.output)
		}

      }
}}}
	
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
