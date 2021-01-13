import groovy.sql.Sql

pipeline {
    agent any
    stages {
       
		stage('Create tabless'){
			 steps{
                script{
				
					def a= sqlconnection().execute'''
						select * from KH255051.departments;
						'''
					println(a)


	
}}}}}

@NonCPS
    def sqlconnection() {
        String URL = "jdbc:teradata://wbtlabserver.teradata.com/KH255051";
        String username = "KH255051";
        String password = "${dbc_password}";
        String driver= "com.teradata.jdbc.TeraDriver"
        
        echo "Building connection"
        def sqlconn = Sql.newInstance(URL, username, password, driver)
        return sqlconn
    }
