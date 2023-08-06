# On boarding a new project on oxidize

 
## Step 1: Knowing and configuring the coding standard in oxidize

Base upon the different client requirement, you may have to configure the different component in oxidize. It is best to discuss this point with client first before creating a new project in oxidize.

### 1. Case option: There are different case options available at the moment which are:
  
<u>Camel Case</u> : This will induce camel case in your table/attribute name. e.g. Table specified as **Writeoff line** in oxidize will be automatically converted to **Writeoff_Line** 

<u>Upper Case</u> : This will induce upper case in your table/attribute name. e.g. table specified as **Writeoff line** in oxidize will be automatically converted to **WRITEOFF_LINE**   

<u>Lower Case</u> : This will induce lower case in your table/attribute name. e.g. table specified as **Writeoff line** in oxidize will be automatically converted to **writeoff_line** 

Based upon the above available case option, you can configure it in your project by including below statment in EJS template:  
For  Camel:
<code>
const codifyOptions = codifier.DatabaseCodifyOptionsCamel </code>

For Upper:  
<code> const codifyOptions = codifier.DatabaseCodifyOptions  </code>

<p style="color:red">Right now we don't have a support for configuring the EJS template in lower case We will be adding the feature in further releases.</p>

### 2. Enforce descriptor: 

Using this configuration user can specify whether the attribute name filled with the descriptor or not.

For example: if in the project you have a field named as **"Direct Written Premium"** and the attribute class of this field is specified as **Currency** and the descriptor of this attribute class is specified as **Amount** then output field name will be something like **Direct_Written_Premium_Amount**

By default this setting is turned on, You can copy and paste below code in your EJS template to turn this setting off.

<code>databaseOptions.enforceDescriptors= false</code>

### 3. Enforce Return Context: 
If a column is referring to another table then the context specified in the column will be used in the column name.

For example: If entity **Written Premium** has attribute as Term which is reference to another entity **Term** and the context specified in the column is **Fnk** then output attribute name will be **Term_Fnk** 

By default this setting is turned on, You can copy and paste below code in your EJS template to turn this setting off.
<code>databaseOptions.enforceReturnContext= false</code>

### 4. Enforce Return Entity: 
If you want to use the domain name before the name of the table then this option can be used.

For example: If entity name is  **Written Premium** and domain of this entity is **Policy**. Now if return entity is turned on(Which is by default) then the Entity name will be like **Policy_Written_Premium**

By default this setting is turned on, You can copy and paste below code in your EJS template to turn this setting off.

<code>databaseOptions.enforceReturnEntity= false</code>  

## Step 2: Creation of a project in oxidize

### To create a project in oxidize go to
https://oxidize.centricconsulting.com/project

Click on create button as specified in below snapshot
![ScreenShot](/doc_snapshot/1.png)

Once clicked you can provide the Name of the project and origin. Afterwhich you can click on Create button to create the project.
![ScreenShot](/doc_snapshot/2.PNG)

## Step 3: Creation of an entity in oxidize

### Go to your project by clicking on the project name
![ScreenShot](/doc_snapshot/3.png)

### Click on create button as specified in below snapshot
![ScreenShot](/doc_snapshot/4.png)

### Provide name of the entity and plural name
Plural name generally used when we are generating star UML diagram. In our case we are using "Transaction" as normal name and "Transactions" as plural name. Now if any table has one-many relationship (one table : Claim and many table: Transaction ) the star UML will use plural name to represent relationships "Transactions".
![ScreenShot](/doc_snapshot/5.png)

### Provide necessary tags to the attribute.
For example in below snapshot you see an entity "transaction" has tagged called 
"domain:Claim" and "Trans:true". 
This means that when we can use this tags to customize DDL and DML statment.
For Example: While generating entity name we can attach a domain. So entity name will be formed like Claim_Transaction.
we have also specified as True in Trans tag which can help us to generate the different bi-temporal template for  table and SP.
![ScreenShot](/doc_snapshot/6.png)

## Step 4: Creation of attribute class in oxidize

Attribute Class is used to define attribute types in a project.
To create a new attribute class You need to follow below step
1. Select your project and go to -> Attribute Class option as shown in below snapshot
    ![ScreenShot](/doc_snapshot/7.png)
    
2. Provide Name of an attribute Class along with definition, descriptor, descriptor variations
    
    The purpose of descriptor is:
    
    Assume a case where you want to put a sufix in every attribute with the attribute class. 
    For example if an attribute name is "coverage" and the class of an attribute is "code".
    Now if you have selected an option to use enforce descriptor as True and if there isn't any descriptor available in attribute class then in such case attribute name in ddl and dml will be "coverage_code".

    Assume a second example where an attribute name is "coverage" and the class of an attribute is "code".
    Now if you have selected an option to use enforce descriptor as True and if there is descriptor available with value as "cd" in attribute class then in such case attribute name in ddl and dml will be "coverage_cd".

    Assume a third example where in output you want an attribute name should be "coverage_txt" where class of attribute is "code" and it has a descriptor as "cd". Now in such cases you can use "descriptor variations" to provide different variations of description. In our case we can put "txt" as variation. So while rendering a template if any of the suffix of attribute is matching with the descriptor variation then it would not going to replace the variations. 

    Below is the summary of the snapshot. How it looks like in oxidize frontend.

    ![ScreenShot](/doc_snapshot/8.png)

3. Now in this step You need to define the premitive data type of your attribut class.
    For Example:

    | Premitive_Type    | Output_Value |
    | --------          | ------- |
    | Text(Precision not set)  | varchar(200)   |
    | Text(Precision 30)  | varchar(30)   |
    | character | CHAR     |
    | boolean    | Bit    |
    | bit    | Bit    |
    | Integer (Preision not specified)    | INT    |
    | Integer (Preision <= 0)    | BIT    |
    | Integer (Preision <= 1)   | SMALLINT    |
    | Integer (Preision <= 2)    | SMALLINT    |
    | Integer (Preision <= 4)    | INT    |
    | Decimal (if attribute class name has currency in it)   | MONEY    |
    | Decimal(Precision =3, scale =7)    | DECIMAL(3,7)    |
    | Decimal(Precision and scale not set)    | DECIMAL(20,8)    |
    | identifier    | VARCHAR(200)    |
    | float (Prevision and scale not set)  | Float(20,8)    |
    | float (Preision =4 scale =3)    | Float(4,3)    |
    | date    | DATE    |
    | time    | DATETIME2(7)    |
    | timestamp   | DATETIME2(7)    |
    | Default    | Varchar(200)    |

Example of How above table can be configured in oxidize front end application
![ScreenShot](/doc_snapshot/9.png)

4. Code Generation  (To be edited based upon input from Jeff )

Tags
suffix:_Dt 
longsuffix:_Date

Configurations

Active


Once you done with this you can save the attribute class by clicking on save button.

## Step 5: Creation of attribute within entity

To Create an attribute within an entity you can
Select Project -> Click on your Entity -> Click on Create button 

Below is snapshot for same
![ScreenShot](/doc_snapshot/10.png)

General: 


![ScreenShot](/doc_snapshot/11.png)

Name: Provide name of attribute

Definition : Provide Decription of your attribute

Derication : Method of derriving a calculated result from input

Grain : If selected it indicates this will uniquely identify records within an entity (To be confirmed with Jeff)

Quality Rule : If you want to validate the input value with some text/regex pattern 
(To be discussed with Jeff)




Return Profile:


![ScreenShot](/doc_snapshot/12.png)

Required : If selected it will not allow null values. DDL data type will defined as not null. (To be confirmed with Jeff)

Mulitple Values :If selected it means it will one to many relationship (To be confirmed with Jeff)

Return Value  : Indicates whether an attribute returns and entity class or Attribute
Attribute Class: If selected you need to provide a attribute class which can use to map the data type of an attribute

Reference : Reference to an entity, This option is used to specify the foreign key.

Context : Primary.... (To be discussed with Jeff)




Code Generation:


![ScreenShot](/doc_snapshot/13.png)

Tags (To be discussed with Jeff)

Configurations (To be discussed with Jeff)

Active (To be discussed with Jeff)




## Step 6: Using predefined list of the templates based on the best suitable in your case
(To be discussed with Jeff if we have the mapping of list of templates available)

## Step 7: Generating the output of oxidize project
There is two way to generate the output of a project.

1 Is to use the oxidize buttton on an oxidize project
You can select a project of your choice for which you want to generate the DDL and DML statement and then click on the oxidize button as highlighted in below snapshot
![ScreenShot](/doc_snapshot/14.png)

2 To use the oxidize sandbox. 

If there is some issue in oxidize application and it is not working as expected, you can use the sandbox version. (You need to contact someone from oxidize support team)

Support Team will download the metadata from the oxidize front end and using the metadata they will render the templates.

To export the metadata you can use below button from the oxidize project
![ScreenShot](/doc_snapshot/15.png)





## Step 8: Excel upload and download to change the metadata within the project.

(To be discussed with Jeff if this functionality works or some issues)



