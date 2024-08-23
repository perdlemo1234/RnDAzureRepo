# Simple / Complex Transformations in Azure Data Factory (ADF)

###### Padlet Link ( Under Azure Data Factory ): https://trello.com/c/BFVrYDY1/14-customise-your-board
- Please refer to the explanation under the padlet link if the descriptions in this ReadMe file are insufficient.
- Sometimes, a YT link from playlist (https://www.youtube.com/playlist?list=PLMWaZteqtEaLTJffbbBzVOv9C0otal1FO) detailing something might be included if Padlet has no existing good explanation. 

### _Introduction:_
###### The main purpose of the documentation is to raise awareness about the many transformations offered in ADF. Some are quite straightforward and some are not. Due to limited space and to encourage simplicity, elobrate explanations are focused onto complex transformations. Very simple transformations are not stated in this documentation. There are also some miscellaneous features to take note as it can be quite confusing or frustrating when trying to understand a simple concept which are slightly different. Also, certain examples like Azure Functions Activity & API call can't be researched further because of PwC permission restrictions. 

### A) _Simple Transformations:_
_1) Filter ( Under filter modifier ):_
###### This selects specific rows from a dataset based on certain conditions. Sometimes, a siple change of data-type is necessary to ensure the "Filter" transformation is working as intended. 

_2) Derived-Column ( Under filter modifier ):_
###### This creates new or edits existing columns by using custom expressions. The following picture attempts to capitalise the names, and also displaying "Yes" or "No" statement based on their salary level - whether salary is above 10000. 
![image](https://github.com/user-attachments/assets/86de5bf1-a8e9-490a-a459-027b74d496dd) 

_3) Conditional-Split ( Under Multiple inputs/outputs ):_
###### Define multiple conditions where it evaluates the multiple rows of incoming data. Condition is based on simple/complex comparisons. Meaning we can create multiple branches of conditions and put it as a. An example is shown below: 
![image](https://github.com/user-attachments/assets/e0de276d-c70f-46a1-96ae-7c564d4532c8)

### _B) Data Merging & Integration Technique:_
_1) Lookup Tranformation ( Under Multiple inputs/outputs ):_ 
###### Think of it as a left outer join. There exist a primary and lookup stream. All outputs disaplyed from primary stream, while only selected rows from lookup stream are included in the final output. PLease refer to trello card for more details under transformation activity.

_2) Union Transformation ( Under Multiple inputs/outputs ):_
###### Involves combining data from multiple sources and consolidate them into a single output. Put it simply, imagine putting all data vertically on top of each other. Order at which union appears matters. PLease refer to trello card for more details under transformation activity

_3) Joined - Column Transformaiton ( Under Multiple inputs/outputs ):_
###### Join columns based on a certain condition. Diagram below shows the different pre-set join types. 
![image](https://github.com/user-attachments/assets/a4950bc2-f145-44e3-afaf-d400bb7c0edf)

### C) _Complex / Hard  Transformations:_
_1) Concatenate Transformation ( Under concatenate_column_transformation ):_
###### Concatenate involves combining rows & columns to form a larger sequence. In-built ADF capabilities allow us to concatenate the columns with ease. Concatenating rows in ADF is possible but will require external services ( Databricks or Azure functions ) to assist with the computation. There are differences between concatenate & derived columns. To put it simply, concatenate is a subset of derived columns - more can be found under the trello card. 

_2) Pivot & Unpivot Transformation ( Under pivot_transformation ):_
###### Pivot : Involves restructuring data from row-based to column based format: 
###### Unpivot : Changes column based format to row format.

###### In both pivot and unpivot transformation, they require the user to enter 3 different inputs: 
###### _1) Group-by / Ungroup by:_
###### _2) Pivot key / Unpivot key:_
###### _3) Pivot column / Unpivot column:_

Check trello document for additional infomation. 

_3) Windows:_
###### Divide the dataset into seperate windows/partitins , and then aggregate to obtain a better/clearer analaysis of the data. 

###### _Would require 4 mandatory inputs ( explained in trello card ):_
1) Over Clause: To create many partitions. **On what basis do I seperate my rows?**
2) Sort: **Order rows in ascending / descending order**
3) Range By: **Not important [Option / Unbounded]**
4) Windows Column: **In each partition, create meaningful analaysis code like (avg(SALARY)) & rank())**
![image](https://github.com/user-attachments/assets/36cf9350-98bf-41fa-8165-23ccd019eadf)

### D) _JSON Transformations (All can be found under "JSON Transformations"):_
_1) FLatten:_
###### https://learn.microsoft.com/en-us/azure/data-factory/data-flow-flatten. Flatten only supports JSON files with arrays in order for the transformation to take place. ADF can automatically which column contains arrays amd provides the option to user which columns to "unroll by". 
![image](https://github.com/user-attachments/assets/f72e90b9-5146-4c12-bcee-f41d89c63f11)

_2) Parse:_
###### https://learn.microsoft.com/en-us/azure/data-factory/data-flow-parse You can think of parsing as a middle man for further processing. (E.G. Columns containing dates in a string format can be parsed into a "Date-Time" data type to perform date & time calculations). It can be more complicated such as extracting parts of data from a string OR converting data from a non-standard format. Examples of other data types are included within the trello documentation. 

_3) Stringify:_
###### https://learn.microsoft.com/en-us/azure/data-factory/data-flow-stringify Involves converting data into string format. Useful for consistent formatting , concatentation , or when integrating with systems which expect string format. For example: if we have a JSON data, then we would want to create a string representation for logging / further processing.
![image](https://github.com/user-attachments/assets/30f02b73-9f89-473a-addd-f1791b12b5a4)

### E) _Miscellaneous Stuff / Stuff To Be Aware Of:_
_1) Pipeline vs Dataflow:_  

## **Pipeline:**
- An orchestrator which directs the flow of data and does not transform data! It also run under Data Factory execution runtime. 
- Consists of various activity types, such as moving data (Copy Activity), running queries (Data Flow Activity) , call external services (Lookup Activitiy and more.)
- Can use parameters to pass values dynamically, allow for flexible execution and reusability.
- Can call data-flows, execute stored procedures, invoke Azure FUnctions or more. So, multiple data-flows can be included within a singular pipeline.
- Triggers can be scheduled to run at specific times or manually to do so or other pipelines run.

## **Dataflow:**
- Dataflow is one of the activity types in pipeline.
- Performs row and column based transformations. It provides a visual interactive interface for review to design & execute transformation. _( examples of all transformations are listed above )_
- At runtime, a Data Flow executed in Spark Environent. 

**NOTE : A pipeline can run without a Datafow ; a dataflow cannot be run without a pipeline.**

_2) Flowlet vs Dataflow:_
## **Flowlet:**
- Reusable component within a Dataflow. Allows us to encampsulate a set of transformations into a reusable module , which can be called multiple data flows.
- Does not require any source or sink to operate. [Has Input & Output] However, flowlet can be created from a dataflow, where the schema will be automatically assigned.
- Expected schema of input & output must be included or else flowlet would not function correctly.
- Use flowlet if we want to reuse common transformation within a Data flow. Helps simplifies data transformaion logic.

## **Dataflow:**
- Same as before, this allows us to design & execute a data transformation through a visual interface. 
- More on designing and executing complex data transformations.

_3) File Name Options (list them down below) (multiple output files?) How to fully utilise them:_

_4) Definition of Partitioning:_

_5) Definition of Aggregation:_

_6) Linked Services:_

_7) Integration Runtimes:_

_8) What kind of output to place at sink? How does it work? What kind of logic must I follow:_

_9) Data Integration: ( Azure SQL Database, Cosmos DB, Azure Blob Storage, and third-party systems )_ 

_10) Security & Access Control: ( Manaage ADF resources & implementing role-based control )_ 




Summarise all transformations. State and explain simple transformations only. _[Filter/change data-type/Derived column/select/lookup/union]_

For the rest of the transformations, state and elaborate further: 
If need additional explanations, please refer to a certain section in the trello card. 
Insert YT links , and website links. 
