import groovy.sql.Sql


output = ""

pipeline {
    agent any
    stages {
       
		stage('Create tabless'){
			 steps{
                script{
		 	sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM employees_final)=100 THEN 1 ELSE 0 END as output") { row ->
				output= "All rows inserted in employees table"+"\t\t\t$row.output"
		}
		
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM departments_final)=4 THEN 1 ELSE 0 END as output") { row ->
				output= "All rows inserted in departments table"+"\t\t\t$row.output"
		}
		
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM jobs_final)=5 THEN 1 ELSE 0 END as output") { row ->
				output= "All rows inserted in jobs table"+"\t\t\t$row.output"
		}
		writeFile(file: 'output.txt', text: output)
		f = new file ("output.txt")
		f.append("newline added!\n")

                  	
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
