# Create Hive External Table
The file ingested by Flume has double-quotes " on all of its values. This can be fixed by adding the SerDe properties seen below
```
CREATE EXTERNAL TABLE default.flume_csv
(
	 department string
	,department_code string
	,employee_identifier int
	,health_dental decimal(18,2)
	,job string
	,job_code string
	,job_family string
	,job_family_code string
	,organization_group string
	,organization_group_code string
	,other_benefits decimal(18,2)
	,other_salaries decimal(18,2)
	,overtime decimal(18,2)
	,retirement decimal(18,2)
	,salaries decimal(18,2)
	,total_benefits decimal(18,2)
	,total_compensation decimal(18,2)
	,total_salary decimal(18,2)
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
Table is now created and ready for analysis
```
select *
from default.flume_csv
limit 10;
```
