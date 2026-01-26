Database Design
01-14-2026

### Data models
Relational models are used in most DBMS today. However, other models do exist'
Relational models state:
- Our data will be organized into tables (similar to an excel spreadsheet)
- These tables (relations) have:
	- Names
	- Attributes (columns)
	- Constraints - The rules that controls what type of data goes where
#### Difference between relational schema and relational instance
Relational schema
- The conceptual design 
	- If you are trying to build a ship you would build a blueprint
	- Then you can build the boat
- Remember relations (tables) have:
	-  Names
	- Attributes (columns)
	- Constraints:
		- setting rules for what valid data is
- The schema is the blueprint of database
- A relation must be a set
- Example of a schema
	- Data is not ordered
	- No duplicates
Relational instance
- The object made following the relational schema 
- i.e:
	- An individual row in a table representing a real person (who might have SSN, age, name, etc.)
 
### Constraints
#### Domain constraints

 If you look at an attribute, the accepted attribute should have the correct input 
	1. Type - Has to be the correct data type
		i.e - SSN Has to be a string
	2.  App specific rules - Has to be from relevant source 
		i.e - Only accepting US SSNs
		i.e - Age cannot be less than 0 or greater than 100
#### Key constraint
- A unique identifier in a dataset
- Super key: 
	- Any kind of unique identifier
- Candidate key:
	- No subset of a candidate key contains a unique identifier
	- In other words it is a unique key in its most minimal form, a unique identifier with little fluff
	- NOTE: if a key is a candidate key it is ALSO a super key
- Primary Key
	- ONLY one
	- The ultra specific key that has no subset and can never be NULL
	- In our example with SSN, name, age, diagnosis SSN will be the best primary key due to it being unique and the smallest.
For example:
	
| **Key Type**      | **Unique?** | **Minimal?** | **Can be NULL?** |
| ----------------- | ----------- | ------------ | ---------------- |
| **Super Key**     | Yes         | No           | Yes              |
| **Candidate Key** | Yes         | Yes          | Usually No*      |
| **Primary Key**   | Yes         | Yes          | **Never**        |

Practice/Example Table

|               | Name | Name, age | SSN, age | age, diag | SSN, name, age |
| ------------- | ---- | --------- | -------- | --------- | -------------- |
| Super key     | N    | Y         | Y        | N         | Y              |
| Candidate Key | N    | Y         | N        | N         | N              |
|               |      |           |          |           |                |


#### Referential constraints
- Foreign Key constraint:
	- Lets say you have two tables
		1. Referencing relation/table
		2. Referenced relation/table
	- A foreign key is if some attribute in table 1 happens to be the primary key in table 2
i.e: 

| This is the instructor table ---> | SID | Course ID |
| --------------------------------- | --- | --------- |


| This is the course table ---> | CID | number of students |
| ----------------------------- | --- | ------------------ |

- In this case an attribute in the instructor table is the key in the course ID.
- In a way a foreign key can be used as a pointer to reference one table through an attribute of another
NOTE: A foreign key can be from the exact same table as the referenced relation/table. The foreign key points to the primary key in its own table.

i.e
- In a table holding data of all employees (SSN, name, age, years at company, supervisor SSN)
- The supervisor SSN will also be in the database as their own employee