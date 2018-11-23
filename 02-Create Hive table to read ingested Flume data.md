# Create Hive External Table
The file ingested by Flume has double-quotes " on all of its values. This can be fixed by adding the SerDe properties seen below. 
```
CREATE EXTERNAL TABLE default.s_employee_compensation
(
	 department string
	,department_code string
	,employee_identifier int
	,health_dental string
	,job string
	,job_code string
	,job_family string
	,job_family_code string
	,organization_group string
	,organization_group_code string
	,other_benefits string
	,other_salaries string
	,overtime string
	,retirement string
	,salaries string
	,total_benefits string
	,total_compensation string
	,total_salary string
	,union string
	,union_code string
	,year int
	,year_type string
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
   "separatorChar" = ",",
   "quoteChar"     = "\""
) 
LOCATION '/user/cloudera/flumecsv';
```
Cast specific columns to be numeric instead of string
```
CREATE TABLE default.employee_compensation
AS
SELECT
	 department as department
	,department_code as department_code
	,employee_identifier as employee_identifier
	,cast(health_dental as decimal(18,2)) as health_dental
	,job as job
	,job_code as job_code
	,job_family as job_family
	,job_family_code as job_family_code
	,organization_group as organization_group
	,organization_group_code as organization_group_code
	,cast(other_benefits as decimal(18,2)) as other_benefits
	,cast(other_salaries as decimal(18,2)) as other_salaries
	,cast(overtime as decimal(18,2)) as overtime
	,cast(retirement as decimal(18,2)) as retirement
	,cast(salaries as decimal(18,2)) as salaries
	,cast(total_benefits as decimal(18,2)) as total_benefits
	,cast(total_compensation as decimal(18,2)) as total_compensation
	,cast(total_salary as decimal(18,2)) as total_salary
	,union as union
	,union_code as union_code
	,cast(year as int) as year
	,year_type as year_type
FROM default.s_employee_compensation;
```
Table is now created and ready for analysis
```
select *
from default.employee_compensation
limit 10;
```
