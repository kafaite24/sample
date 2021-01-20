import groovy.sql.Sql


output = ""
flag = 0
pipeline {
    agent any
    stages {
       
		stage('Run test cases'){
			 steps{
				 
                script{
		
		 	sqlconnection().eachRow("select case when (select count(*) from employees_staging where DateAdded= current_timestamp) = (select count(*) from employees_landing ) then 'Passed' else 'Failed' end as output") { row ->
				output= "All rows inserted in employees table"+"\t\t\t$row.output\n\n"
		}
			if("${output}"=='Failed'){
				flag=1
			}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("select case when (select count(*) from departments_staging where DateAdded= current_timestamp) = (select count(*) from departments_landing ) then 'Passed' else 'Failed' end as output") { row ->
				output += "All rows inserted in departments table"+"\t\t\t$row.output\n\n"
		}
			if("${output}"=='Failed'){
				flag=1
			}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("select case when (select count(*) from jobs_staging where DateAdded= current_timestamp) = (select count(*) from jobs_landing ) then 'Passed' else 'Failed' end as output") { row ->
				output += "All rows inserted in jobs table      "+"\t\t\t$row.output\n\n"
		}
			if("${output}"=='Failed'){
				flag=1
			}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (select sum(duplicates) from (select count(*) as duplicates from employees_staging group by employee_id HAVING COUNT(*) > 1) as output) is not null THEN 'Passed' else 'Failed' END as output") { row ->
				output += "No duplicate PK in employees table   "+"\t\t\t$row.output\n\n"
		}
			if("${output}"=='Failed'){
				flag=1
			}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (select sum(duplicates) from (select count(*) as duplicates from departments_staging group by department_id HAVING COUNT(*) > 1) as output) is not null THEN 'Passed' else 'Failed' END as output") { row ->
				output += "No duplicate PK in departments table   "+"\t\t\t$row.output\n\n"
		}
			if("${output}"=='Failed'){
				flag=1
			}
			writeFile(file: 'output.txt', text: output)
                  	
			sqlconnection().eachRow("SELECT CASE WHEN (select sum(duplicates) from (select count(*) as duplicates from jobs_staging group by job_id HAVING COUNT(*) > 1) as output) is not null THEN 'Passed' else 'Failed' END as output") { row ->
				output += "No duplicate PK in jobs table   "+"\t\t\t$row.output\n\n"
		}
			if("${output}"=='Failed'){
				flag=1
			}
			writeFile(file: 'output.txt', text: output)
			
			println("${flag}")
			
			if("${flag}"==1){
				error('Build Failed')
			}
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
