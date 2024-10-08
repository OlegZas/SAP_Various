# **Description of the Procedure**

The SQL procedure **NEWSCHEMA.EXTRACT_PERSONNEL** is designed to extract personnel data from a source database and populate a target table named **NEWSCHEMA.PERSONNEL**. Here’s a breakdown of what the procedure does:

## **1. Table Definition**

The **NEWSCHEMA.PERSONNEL** table is created with a variety of columns to store personnel information, including:

- **ORGID**: Organizational identifier, defaulting to the session user.
- **WORKERID**: Unique identifier for each worker, marked as not nullable.
- **IDENTIFIER**: Another unique identifier for the worker.
- **STARTDATE** & **ENDDATE**: Dates indicating the employment period.
- **TITLE**, **FIRSTNAME**, **MIDDLENAME**, **LASTNAME**, **SUFFIX**: Name details of the personnel.
- **ISACTIVE**: A flag indicating if the worker is currently active.
- **CREATIONDATE** & **DELETIONDATE**: Timestamps for record creation and deletion.
- **SOCIALSECURITYID**: Worker’s social security identifier.
- **ANNUALSALARY**: Salary details.
- **DATEHIRED** & **DATELEFT**: Dates related to the hiring and leaving of the employee.
- **DEPARTMENTNAME**: The name of the department the worker belongs to.
- **Attributes and Numbers**: A series of generic attributes and number fields to store additional information.

## **2. Procedure Definition**

The procedure starts with two parameters:

- **FILENAME**: An output parameter to store the generated filename.
- **RUNID**: An input parameter used to identify the specific run for extracting data.

## **3. Variable Declaration**

- **V_PERIODID**: Holds the period ID related to the provided RUNID.
- **V_YESTERDAY**: Stores the date for yesterday, calculated from the current UTC date.

## **4. Fetch PERIODID**

The procedure retrieves the **PERIODID** from the **DATA_RUN** table that matches the provided **RUNID**. This ID can be useful for tracking or managing the data extraction process.

## **5. Truncate Target Table**

The procedure truncates the **NEWSCHEMA.PERSONNEL** table to remove any existing records, ensuring that the table is empty before importing new data.

## **6. Filename Generation**

A filename for the extracted data is created using the current date and time, formatted as **PERSONNEL_YYYYMMDD_HH24MISS.csv**.

## **7. Data Insertion**

The procedure then inserts records into the **NEWSCHEMA.PERSONNEL** table by selecting data from the **SOURCE.PARTICIPANTS** table. This selection includes:

- Joining with the **SOURCE.PAYEES** table to ensure only active records are fetched.
- A left join with the **SOURCE.DEPARTMENTS** table to include department names.
- The **WHERE** clause filters for active personnel records.

## **8. Data Selection**

The selected columns for insertion include all relevant details such as names, dates, identifiers, and attributes.

## **Summary**

Overall, this procedure is an automated **ETL (Extract, Transform, Load)** process that:

- Initializes an empty personnel table.
- Retrieves relevant personnel data based on a specific run ID.
- Inserts the filtered data from source tables into the target personnel table.

This setup ensures that the personnel data is up-to-date and accurately reflects the current state of the organization’s workforce.

