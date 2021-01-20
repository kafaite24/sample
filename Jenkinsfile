import groovy.sql.Sql


output = ""
flag = 0
pipeline {
    agent any
    stages {
       
		stage('Run test cases'){
			 steps{
				 
                script{
		
		 	sqlconnection().eachRow("select case when (select count(*) from employees_staging where DateAdded in (select DateAdded from employees_landing)) = (select count(*) from employees_landing) then 'Passed' else 'Failed' end as output") { row ->
				output= "All rows inserted in employees table"+"\t\t\t$row.output\n\n"
				
				if("${row.output}"=="Failed"){
					flag=1}
				
				println("${row.output}")
		}
			println("${flag}")
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("select case when (select count(*) from departments_staging where DateAdded in (select DateAdded from departments_landing)) = (select count(*) from departments_landing) then 'Passed' else 'Failed' end as output") { row ->
				output += "All rows inserted in departments table"+"\t\t\t$row.output\n\n"
		
				if("${row.output}"=="Failed"){
					flag=1}
			}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("select case when (select count(*) from jobs_staging where DateAdded in (select DateAdded from jobs_landing)) = (select count(*) from jobs_landing) then 'Passed' else 'Failed' end as output") { row ->
				output += "All rows inserted in jobs table      "+"\t\t\t$row.output\n\n"
				
				if("${row.output}"=="Failed"){
					flag=1}
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("select case when (select count(gender) from employees_view where gender in ('Male', 'Female'))=0 then 'Passed' else 'Failed' end as output") { row ->
				output += "Gender column updated     "+"\t\t\t\t$row.output\n\n"
				
				if("${row.output}"=="Failed"){
					flag=1}
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (SELECT count(*) FROM employees_view where name_prefix Is null or first_name is null or last_name is null or gender is null or email is null or father_name is null or mother_name is null or age is null or weight is null or age_in_company is null or salary is null or SSN is null or phone_number is null or country is null or city is null or state is null or region is null or zip_code is null or username is null or passcode is null or job_id is null or manager_id is null or department_id is null or quarter_of_joining is null)=0 then 'Passed' else 'Failed' end as output") { row ->
				output += "No null values     "+"\t\t\t\t\t$row.output\n\n"
				
				if("${row.output}"=="Failed"){
					flag=1}
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (select sum(duplicates) from (select count(*) as duplicates from employees_staging group by employee_id HAVING COUNT(*) > 1) as output) is null THEN 'Passed' else 'Failed' END as output") { row ->
				output += "No duplicate PK in employees table   "+"\t\t\t$row.output\n\n"
				
				if("${row.output}"=="Failed"){
					flag=1}
		}
			writeFile(file: 'output.txt', text: output)
			
			sqlconnection().eachRow("SELECT CASE WHEN (select sum(duplicates) from (select count(*) as duplicates from departments_staging group by department_id HAVING COUNT(*) > 1) as output) is null THEN 'Passed' else 'Failed' END as output") { row ->
				output += "No duplicate PK in departments table   "+"\t\t\t$row.output\n\n"
				
				if("${row.output}"=="Failed"){
					flag=1}
		}
			writeFile(file: 'output.txt', text: output)
                  	
			sqlconnection().eachRow("SELECT CASE WHEN (select sum(duplicates) from (select count(*) as duplicates from jobs_staging group by job_id HAVING COUNT(*) > 1) as output) is null THEN 'Passed' else 'Failed' END as output") { row ->
				output += "No duplicate PK in jobs table   "+"\t\t\t$row.output\n\n"
				
				if("${row.output}"=="Failed"){
					flag=1}
		}
			writeFile(file: 'output.txt', text: output)
			
			if("${flag}"=="1"){
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
