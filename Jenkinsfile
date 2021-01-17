import groovy.sql.Sql


output = ""
sqlconn= null
@NonCPS
    def sqlconnection() {
        String URL = "jdbc:teradata://wbtlabserver.teradata.com/KH255051";
        String username = "KH255051";
        String password = "bSji5jkv";
        String driver= "com.teradata.jdbc.TeraDriver"
        
        echo "Building connection"
        sqlconn = Sql.newInstance(URL, username, password, driver)
    }

pipeline {
    agent any
	
    
    stages {
	  
		stage('database'){
			steps{
				sqlconnection()
		}}
		stage('Create tabless'){
			 steps{
                script{
			println(${sqlconn})
					
				sqlconn.close()     	
      }
}}}
}

