# Simple / Complex Transformations in Azure Data Factory (ADF)

###### Padlet Link ( Under Azure Data Factory ): https://trello.com/c/BFVrYDY1/14-customise-your-board
- Please refer to the explanation under the padlet link if the descriptions in this ReadMe file are insufficient.
- Sometimes, a YT link from playlist (https://www.youtube.com/playlist?list=PLMWaZteqtEaLTJffbbBzVOv9C0otal1FO) detailing something might be included if Padlet has no existing good explanation. 

### _Introduction:_
###### The main purpose of the documentation is to raise awareness about the many transformations offered in ADF. Some are quite straightforward and some are not. Due to limited space and to encourage simplicity, elobrate explanations are focused onto complex transformations. Very simple transformations are not stated in this documentation. There are also some miscellaneous features to take note as it can be quite confusing or frustrating when trying to understand a simple concept which are slightly different. Also, certain examples like Azure Functions Activity & API call can't be researched further because of PwC permission restrictions. 

### A) _Simple Transformations:_
---------
_1) Filter ( Under filter modifier ):_
###### This selects specific rows from a dataset based on certain conditions. Sometimes, a siple change of data-type is necessary to ensure the "Filter" transformation is working as intended. 
---------
_2) Derived-Column ( Under filter modifier ):_
###### This creates new or edits existing columns by using custom expressions. The following picture attempts to capitalise the names, and also displaying "Yes" or "No" statement based on their salary level - whether salary is above 10000. 
![image](https://github.com/user-attachments/assets/86de5bf1-a8e9-490a-a459-027b74d496dd) 
---------
_3) Conditional-Split ( Under Multiple inputs/outputs ):_
###### Define multiple conditions where it evaluates the multiple rows of incoming data. Condition is based on simple/complex comparisons. Meaning we can create multiple branches of conditions and put it as a. An example is shown below: 
![image](https://github.com/user-attachments/assets/e0de276d-c70f-46a1-96ae-7c564d4532c8)
---------
### _B) Data Merging & Integration Technique:_
---------
_1) Lookup Tranformation ( Under Multiple inputs/outputs ):_ 
###### Think of it as a left outer join. There exist a primary and lookup stream. All outputs disaplyed from primary stream, while only selected rows from lookup stream are included in the final output. PLease refer to trello card for more details under transformation activity.
---------
_2) Union Transformation ( Under Multiple inputs/outputs ):_
###### Involves combining data from multiple sources and consolidate them into a single output. Put it simply, imagine putting all data vertically on top of each other. Order at which union appears matters. PLease refer to trello card for more details under transformation activity
---------
_3) Joined - Column Transformaiton ( Under Multiple inputs/outputs ):_
###### Join columns based on a certain condition. Diagram below shows the different pre-set join types. 
![image](https://github.com/user-attachments/assets/a4950bc2-f145-44e3-afaf-d400bb7c0edf)
---------
### C) _Complex / Hard  Transformations:_
---------
_1) Concatenate Transformation ( Under concatenate_column_transformation ):_
###### Concatenate involves combining rows & columns to form a larger sequence. In-built ADF capabilities allow us to concatenate the columns with ease. Concatenating rows in ADF is possible but will require external services ( Databricks or Azure functions ) to assist with the computation. There are differences between concatenate & derived columns. To put it simply, concatenate is a subset of derived columns - more can be found under the trello card. 
---------
_2) Pivot & Unpivot Transformation ( Under pivot_transformation ):_
###### Pivot : Involves restructuring data from row-based to column based format: 
###### Unpivot : Changes column based format to row format.

###### In both pivot and unpivot transformation, they require the user to enter 3 different inputs: 
_1) Group-by / Ungroup by:_  
_2) Pivot key / Unpivot key:_  
_3) Pivot column / Unpivot column:_

###### Check trello document for additional infomation. 
---------
_3) Windows:_
###### Divide the dataset into seperate windows/partitins , and then aggregate to obtain a better/clearer analaysis of the data. 

###### _Would require 4 mandatory inputs ( explained in trello card ):_
1) Over Clause: To create many partitions. **On what basis do I seperate my rows?**
2) Sort: **Order rows in ascending / descending order**
3) Range By: **Not important [Option / Unbounded]**
4) Windows Column: **In each partition, create meaningful analaysis code like (avg(SALARY)) & rank())**
![image](https://github.com/user-attachments/assets/36cf9350-98bf-41fa-8165-23ccd019eadf)

---------

### D) _JSON Transformations (All can be found under "JSON Transformations"):_
---------
_1) FLatten:_
###### https://learn.microsoft.com/en-us/azure/data-factory/data-flow-flatten. Flatten only supports JSON files with arrays in order for the transformation to take place. ADF can automatically which column contains arrays amd provides the option to user which columns to "unroll by". 
![image](https://github.com/user-attachments/assets/f72e90b9-5146-4c12-bcee-f41d89c63f11)
---------
_2) Parse:_
###### https://learn.microsoft.com/en-us/azure/data-factory/data-flow-parse You can think of parsing as a middle man for further processing. (E.G. Columns containing dates in a string format can be parsed into a "Date-Time" data type to perform date & time calculations). It can be more complicated such as extracting parts of data from a string OR converting data from a non-standard format. Examples of other data types are included within the trello documentation. 
---------
_3) Stringify:_
###### https://learn.microsoft.com/en-us/azure/data-factory/data-flow-stringify Involves converting data into string format. Useful for consistent formatting , concatentation , or when integrating with systems which expect string format. For example: if we have a JSON data, then we would want to create a string representation for logging / further processing.
![image](https://github.com/user-attachments/assets/30f02b73-9f89-473a-addd-f1791b12b5a4)
---------
### E) _Miscellaneous Stuff / Stuff To Be Aware Of:_
---------
_1) Pipeline vs Dataflow:_  

##### **Pipeline:**
- An orchestrator which directs the flow of data and does not transform data! It also run under Data Factory execution runtime. 
- Consists of various activity types, such as moving data (Copy Activity), running queries (Data Flow Activity) , call external services (Lookup Activitiy and more.)
- Can use parameters to pass values dynamically, allow for flexible execution and reusability.
- Can call data-flows, execute stored procedures, invoke Azure FUnctions or more. So, multiple data-flows can be included within a singular pipeline.
- Triggers can be scheduled to run at specific times or manually to do so or other pipelines run.

##### **Dataflow:**
- Dataflow is one of the activity types in pipeline.
- Performs row and column based transformations. It provides a visual interactive interface for review to design & execute transformation. _( examples of all transformations are listed above )_
- At runtime, a Data Flow executed in Spark Environent. 

**NOTE : A pipeline can run without a Datafow ; a dataflow cannot be run without a pipeline.**
---------
_2) Flowlet vs Dataflow:_
##### **Flowlet:**
- Reusable component within a Dataflow. Allows us to encampsulate a set of transformations into a reusable module , which can be called multiple data flows.
- Does not require any source or sink to operate. [Has Input & Output] However, flowlet can be created from a dataflow, where the schema will be automatically assigned.
- Expected schema of input & output must be included or else flowlet would not function correctly.
- Use flowlet if we want to reuse common transformation within a Data flow. Helps simplifies data transformaion logic.

##### **Dataflow:**
- Same as before, this allows us to design & execute a data transformation through a visual interface. 
- More on designing and executing complex data transformations.
---------
_3) File Name Options (List them down below) ( ( How to fully utilise them? ):_
##### _**a) "Pattern"**_
- Specify naming pattern for output files using expressions.
- Example of file naming pattern : "data_{YYYYMMDD}.csv"

##### _**b) Per Partition**_
- Used to create a seperate file for each data partition when outputting data.
- To handle large datasets by splitting them into smaller manageable files based on specific partition criteria such as date-ranges, categories, or regions. 

##### _**c) Name File as Column Data:**_
![image](https://github.com/user-attachments/assets/7446c0b6-c03e-47fa-8422-b0f4f3a4ef67)
- Use the content of a specific column , which can be useful for organising files based on data attributes
- ADF will create seperate files where _filename_ is **determined by data in specified column.** For _every unique value_ in the column, a seperate file is created. **E.G.** If **"Region"** column is selected with **'NorthAmerica', 'Europe'** will result in file names like **"NorthAmerica.csv"** , **"Europe.csv"**
- https://stackoverflow.com/questions/67106031/azure-data-factory-name-file-as-column-data-option-in-sink-transformation-of-d <- refer to this document for additional information.
- **Focuses** on **generating filenames** based on column values, leading to multiple files where filename reflects the data attribute. 

##### _**d) Name Folder as Column Data**_
- Similar to "Name File as Column Data", where ADF will also create the correct corresponding folder as well as the files within that folder.
- Best used when **organising files** into a **directory structure** is beneficial, when dealing with large datasets. 

##### **e) Output to Single File**
- Configures activity to consolidate all output data into _**ONE SINGLE FILE**_
- This implies that the other "file-naming" conventions will generally lead to multiple files being created 
---------
_4) Linked Services:_
- A linked service is a connection string to various external sources / destinations, acting as a bridge between ADF and all available data sources.
- Allows ADF TO move and transform data between data sources like ( ADLS, ABS , Azure Cosmos DB, Azure Table Storage ) 
---------
_5) Integration Runtimes vs Triggers:_
##### **_IR ( Integration Runtime ):_**
- Can be utilised within dataflow and pipeline.
- There are dataflows within ADF for visual data transformation & integration. When running these transformations, **AIR** is responsible for all of these transformations because they have the necessary resources to complete it. 

##### _**Triggers:**_
- Initiate the execution of the pipeline in ADF.
- They schedule an event or start the pipeline 
---------
_6) Inline vs Local Dataset:_  
Both methods of source-type will require some use case of linked service. 
##### **_Inline:_**
- ADF datasets defined directly with pipeline activities rather than being pre-defined and stable.
- Often used for one time/simple data transformation/activity

##### **_Local:_**
- Pre-defined dataset created & managed seperately from the pipeline activities.
- Reusable across multiple pipelines.
- Usually used if dataset requires repeated usage.  
---------
_7) What kind of output to place at sink? How does it work? What kind of logic must I follow?:_
- In most cases, the sink will infer the schema from the source.
- We can create a **".xxx file"** which directs ONLY to the directory of the sink. _( Think of it like a blank slate for the transformation to absorb. )_
- ADF will automatically create and update new schema for the specfic transformation within the data pipeline.
- Can apply dynamic content - if we want to write data to different locations based on parameters or variables. 
![image](https://github.com/user-attachments/assets/04767254-8e81-47f5-9cbb-3945c5018b7d)
---------
_8) Azure Functions vs Azure Databircks:_
Both functions and Databricks involve some substantial amount of coding but what exactly are the difference and how do we fully utilise them? 
##### **_Azure Functions:_**
- A _**serverless**_ compute serivce provided by Microsoft Azure - allows us to run code without having to manage underlying infrastructure.
- Able to write in various languages , including C#, Javascript , Python, Powershell.
- Can be triggered by variety of events such as HTTP requests, timer schedules, or changes in data.
- Intended for lightweight operations , and small event-driven tasks.  

##### **_Azure Databricks:_**
- Requires a server to run the code. Think of it like a Jupyter Notebook but not exactly!
- Is an Apaache Spark-based analytics platform optimised for Azure. Has sharing features and a collaborative environment for data engineering, data science, & machine learning.
- Optimised for large scale data procesing 
---------
_9) Data Integration: ( Azure SQL Database, Cosmos DB, Azure Blob Storage, and third-party systems)_ 
_Do I need to include this????_

_10) Security & Access Control: ( Manaage ADF resources & implementing role-based control )_ 
_Do I need to include this????_

Summarise all transformations. State and explain simple transformations only. _[Filter/change data-type/Derived column/select/lookup/union]_

_For the rest of the transformations, state and elaborate further: 
If need additional explanations, please refer to a certain section in the trello card. 
Insert YT links , and website links._
