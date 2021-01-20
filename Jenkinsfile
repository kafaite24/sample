import groovy.sql.Sql


output = ""

pipeline {
    agent any
    stages {
       
		stage('Create tabless'){
			 steps{
				 
                script{
		
		 	sqlconnection().eachRow("select case when (select count(*) from employees_staging where DateAdded= current_date) = (select count(*) from employees_landing ) then 'Passed' else 'Failed' end as output") { row ->
				output= "All rows inserted in employees table"+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("select case when (select count(*) from departments_staging where DateAdded= current_date) = (select count(*) from departments_landing ) then 'Passed' else 'Failed' end as output") { row ->
				output += "All rows inserted in departments table"+"\t\t\t$row.output\n"
		}
			
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("select case when (select count(*) from jobs_staging where DateAdded= current_date) = (select count(*) from jobs_landing ) then 'Passed' else 'Failed' end as output") { row ->
				output += "All rows inserted in jobs table      "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT COUNT(*) FROM employees_prod GROUP BY employee_id HAVING COUNT(*) > 1) is null THEN 'Passed' else 'Failed' END as output") { row ->
				output += "No duplicate PK in employees table   "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT COUNT(*) FROM departments_prod GROUP BY department_id HAVING COUNT(*) > 1) is null THEN 'Passed' else 'Failed' END as output") { row ->
				output += "No duplicate PK in departments table   "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
                  	
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT COUNT(*) FROM jobs_prod GROUP BY job_id HAVING COUNT(*) > 1) is null THEN 'Passed' else 'Failed' END as output") { row ->
				output += "No duplicate PK in jobs table   "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM employees_prod where name_prefix Is null or first_name is null or last_name is null or gender is null or email is null or father_name is null or mother_name is null or age is null or weight is null or age_in_company is null or salary is null or SSN is null or phone_number is null or country is null or city is null or state is null or region is null or zip_code is null or username is null or passcode is null or job_id is null or manager_id is null or department_id is null or quarter_of_joining is null)=0 then 'Passed' else 'Failed' end as output") { row ->
				output += "No nulls in employees prod table   "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM departments_prod WHERE department_id is null or department_name is null or location is null)=0 then 'Passed' else 'Failed' end as output") { row ->
				output += "No nulls in departments prod table   "+"\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM jobs_prod WHERE job_id is null or job_title is null or min_salary is null or max_salary is null)=0 then 'Passed' else 'Failed' end as output") { row ->
				output += "No nulls in jobs prod table   "+"\t\t\t\t$row.output\n"
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("select case when (select count(gender) from employees_final where gender in ('Male', 'Female'))=0 then 'Passed' else 'Failed' end as output") { row ->
				output += "Gender column values changed to 'M' and 'F'"+"\t\t$row.output\n"
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
