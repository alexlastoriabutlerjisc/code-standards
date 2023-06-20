# SQL code standardisation

## Contents
- Purpose
- Standards
  - Naming objects
  - Line length
  - Text case
  - Commas
  - Indenting
  - Line breaks
  - Alias
  - Joins
  - Field/table square brackets
  - Else in case statements
  - Commenting
- Other
- Best practice
- Writing tables
- Full example script

### Purpose
The objective of this document is to build the framework of efficiently written, consistent SQL code, the result being faster authoring and peer review of that code.
While it is not the intention to create a ‘code farm’ mentality within the company, this document will align with RAP’s recommendation of consistent, readable and well-structured code, the benefit of which that this code can form a repository, used to compile code in a “jigsaw puzzle” fashion. 
It is worth noting that this document is not intended as a ‘how-to’ or best practices guide, while very closely related, standards are an overlay once understanding of the code is in place.

### Standards

#### Naming objects
- Avoid the name of a table/column in the plural. It is better use Subject instead of Subjects.
- If the name of the table or column must consist of more than one word, use CamelCase style instead, for example NumberOfSubject. The preferred style is different for different relational database systems. An underscore can be used to merge the two or more words for example Number_Of_Subject. 
- Check that the name is not already used as a keyword in SQL (the text will change colout from black).
- If the name is the same as an SQL keyword, enclose the name within quotation marks.
- The name of an object in a database for a table or a column should be unique and not too long. Avoid punctuation characters and special characters in the name like $, &, * etc. (use only letters, numbers, underscores and greatly minimise the use of ‘spaces’ ).
- Use an underscore in a name only if necessary.
- Don't start the name with an underscore.
- Use comments only if necessary.
- Avoid abbreviations, but, if you do use them, be sure that they will be understood.
- Avoid giving the same name to both a table and a column.
- Use the same naming rules for aliases for columns or tables.
- Include the AS keyword for creating aliases, because this makes the code more readable.
- For the primary key column avoid the name id. A good idea is to combine id with the name of a table, for example: IdUKPRN or DK_Instance.

#### Line length
Code should fit into a standard 1080 pixel width resolution screen. 160 characters should be the maximum line length. This prevents needing to scroll both vertically and horizontally.

#### Text case
- Upper case for reserved/key words (blue, pink, grey) 
- Lower case for other (black) text for readability, e.g. queried variable names. Aliased fields should be _______ appropriate for context e.g. SEC?

#### Commas
Commas before variable names is recommended for in-line editing.

#### Indenting
- Recommended for lines between SELECT and FROM, 1 indent (either tab or 4 spaces, consistently)
- Indent once for WHEN after CASE e.g.
```sql
	,CASE 
		WHEN PR.INSTID = '0001' THEN 'The Open University'
		WHEN Stu.F_UKPRN = '10066143' THEN 'Paris Dauphine International' 
		ELSE PR.ProviderName
		END
	 AS [HE provider] 
```

```sql
,CASE
	WHEN S.F_BIRTHDTE IS NULL OR S.F_COMDATE IS NULL THEN 'U'
	ELSE CAST(DATEDIFF(YY,S.F_BIRTHDTE,S.F_COMDATE) - 
	CASE
		WHEN DATEADD(YY,DATEDIFF(YY,S.F_BIRTHDTE,S.F_COMDATE),S.F_BIRTHDTE) > S.F_COMDATE THEN 1
		ELSE 0
	END AS VARCHAR(40))
END AS F_ZAGEONENTRY
```


- It is recommended to put END and AS on separate lines to make it quicker to copy the statement to the GROUP BY
- Subqueries should be indented once from the current code block e.g.
 
#### Line breaks
Start a new line after: 
```SELECT``` and each field following
```CASE``` and for each ```WHEN```
e.g. 
```sql
FROM …
JOIN …
ON …
```
e.g.
 
INTO …

#### Alias
- AS [____] for field names, no need to AS for temporary table names. Recommend no more than 3 characters. These aliases should be uppercase.
- ‘___ = ’ should not be used to name fields as equals should be reserved for value comparative functions.
- For permanent or semi-permanent tables (e.g. INTO) use [dbo].[XXX_YYYYYYYY] where XXX is your initials and YYYYYYYY is a descriptive and commonly understood table name.
- Alphanumerics only with hypens, underscores
- It is recommend to alias fields in line with open data field names, this reduces use of spaces and enforces upper case field names.
- If the alias changes colour from black, use a different alias, keywords should not be used for field names.
- To make importing data into R, it is recommended not to use spaces in field names and aliases as R typically replaces spaces in variable names with full stops.
- Only use letters, numbers and underscores in aliases i.e. no commas, hyphens etc that will cause issues with exported data.
- For simple aggregate columns, use the same name as the column in the formula e.g. SUM(FPE.FullPersonEquivalent) AS FullPersonEquivalent  

#### Joins
When more than one table is in use, ensure the alias is applied in line with the standards above. All selected fields in the query should be prefixed with the source table alias.

#### Field/table square brackets
Should be used consistently throughout, however they are preferred to be used to quickly identify fields in the code at a glance.

#### Else in case statements
- It is recommended to use the base field value concatenated with a prefix of “error” in the ELSE of a case statement. This allows for easy identification and debugging of data issues.

#### Commenting
- Use as much as possible. 
- Recommended to write before starting the code to explain each step of the intended outcome. 
- Comments should explain why something is happening, not particularly what is happening.
- Use /*   */ for outside of code blocks -- for inline, however this tends to interfere with copy/pasting.
E.g.
```sql
/* Base dataset containing year, mode etc */
SELECT
CAST(FPE.CollectionYear AS CHAR(4)) + '/' + SUBSTRING(CAST(FPE.CollectionYear + 1 AS CHAR(4)), 3, 4) AS [Academic Year]
,CASE -- Grouped mode
	WHEN MS.Mode3WayCategoryCode = '1' THEN 'Full-time (including sandwich)' 
	WHEN MS.Mode3WayCategoryCode = '2' THEN 'Part-time'
ELSE 'Check'
END AS [Mode of study] 
```

#### Other
- Remove whitespace from line ends and between lines
- Use a semicolon ; to end statements, SQL prompt can work around this but it is not a default.
- Separate statements with a consistent number of line breaks

### Best practice

#### Writing tables
Efficiency and data minimisation should be the first consideration when extracting data. As such, when writing extract code, consideration should be made for the size of tables that are being queried, i.e. number of years stored including archived ‘cuts’ and how long this will take to extract, and, the storage used in both the short and long term storage and processing time of the server.

Always tidy any temporary tables created in the procedure and look to make it easier to identify and remove physical (INTO) tables once they are no longer needed.
CTE can be a useful method to store data in memory that is then usable in subsequent queries within the script without creating temporary physical tables.

dbo.[FS]____
date

 
### Appendix

Full example script

Some of the indenting formatting below has changed in Word.
```sql
SELECT 
	 CAST(FPE.CollectionYear AS CHAR(4)) + '/' + SUBSTRING(CAST(FPE.CollectionYear + 1 AS CHAR(4)), 3, 4) AS [Academic Year]
    ,CASE
        WHEN FPE.StudentHomeOverseas = 2
			AND DC.DomicileCode = 'GREU' THEN 'Other European Union'
        WHEN FPE.StudentHomeOverseas = 2 THEN 'Non-European Union'
        WHEN DC.DomicileCode IN ( 'XK', 'Z' ) THEN 'Other UK'
        WHEN DC.DomicileCode IN ( 'A', 'B', 'D', 'E', 'F', 'G', 'H', 'J', 'K', 'XF' ) THEN 'England'
        WHEN FPE.StudentHomeOverseas = 3 THEN 'Not known'
        ELSE DC.DW_Region_Domicile
	 END
	 AS [F_DOMICILE]
    ,SJB.SubjectPrincipleLabel_OpData AS [Principal subject]
    ,CASE
        WHEN SJB.SubjectPrincipleCode = '26-01-03' THEN CONCAT(SJB.SubjectAreaCode, ' ', 'Geography, earth and environmental studies (social sciences)')
        WHEN SJB.SubjectAreaCode = '26' THEN CONCAT(SJB.SubjectAreaCode, ' ', 'Geography, earth and environmental studies (natural sciences)')
        ELSE CONCAT(SJB.SubjectAreaCode, ' ', SJB.SubjectArea)
     END 
	 AS [Subject area]
    ,SJB.DW_ScienceMarker AS [Science Marker]
    ,SJB.SubjectLabel_OpData AS [4-digit JACS subject]
    ,CASE
        WHEN FPE.FirstYearOfStudy = '1' THEN 'First year'
        WHEN FPE.FirstYearOfStudy = '2' THEN 'Other years'
        ELSE 'Check'
     END 
	 AS [First year marker]
    ,DQ.QualTo5Level AS [Level of study]
    ,CASE
        WHEN FPE.QualificationObtainedModeOfStudy = 1 THEN 'Full-time'
        WHEN FPE.QualificationObtainedModeOfStudy = 2 THEN 'Part-time'
        ELSE 'Not Known'
     END 
	 AS [Mode of study]
    ,SUM(FPE.FullPersonEquivalent) AS [FullPersonEquivalent]
FROM Exploitation.F_FullPersonEquivalent AS FPE
    JOIN Exploitation.D_Provider AS PO
        ON PO.S_Provider = FPE.S_Provider
    JOIN Exploitation.D_Subject SJA
        ON SJA.S_Subject = FPE.S_Subject
    JOIN Exploitation.D_Subject SJB
        ON SJB.SubjectCode = SJA.SubjectCode
           AND SJB.DW_FromDate <= 20200801
           AND SJB.DW_ToDate >= 20200801
    JOIN Exploitation.D_Provider AS P
        ON P.DK_Provider = PO.DK_Provider
           AND P.DW_ToDate = 99999999
    JOIN Exploitation.D_Qualification DQ
        ON DQ.S_Qualification = FPE.S_QualificationObtained
    JOIN Exploitation.D_Domicile DC
        ON DC.S_Domicile = FPE.S_Domicile
    JOIN Exploitation.D_Subject AS SJ
        ON SJ.S_Subject = FPE.S_Subject
    JOIN Exploitation.D_ModeOfStudy AS MS
        ON MS.S_Mode = FPE.S_ModeOfStudy
WHERE 
	FPE.DW_FromDate >= (
        SELECT MAX(Exploitation.F_FullPersonEquivalent.DW_FromDate)
        FROM Exploitation.F_FullPersonEquivalent) - 10000
	AND FPE.DW_CollectionGroup IN ( 51, 54 )
	AND FPE.QualificationsObtainedPopulation = 1
GROUP BY 
	 CAST(FPE.CollectionYear AS CHAR(4)) + '/' + SUBSTRING(CAST(FPE.CollectionYear + 1 AS CHAR(4)), 3, 4)
    ,SJB.SubjectPrincipleLabel_OpData
    ,CASE
        WHEN SJB.SubjectPrincipleCode = '26-01-03' THEN CONCAT(SJB.SubjectAreaCode, ' ', 'Geography, earth and environmental studies (social sciences)')
        WHEN SJB.SubjectAreaCode = '26' THEN CONCAT(SJB.SubjectAreaCode, ' ', 'Geography, earth and environmental studies (natural sciences)')
        ELSE CONCAT(SJB.SubjectAreaCode, ' ', SJB.SubjectArea)
    END
    ,SJB.DW_ScienceMarker
    ,SJB.SubjectLabel_OpData
    ,DQ.QualTo5Level
    ,FPE.QualificationObtainedModeOfStudy
    ,FPE.StudentHomeOverseas
    ,DC.DomicileCode
    ,DC.DW_Region_Domicile
    ,FPE.FirstYearOfStudy;
```


ChatGPT’s take on SQL standards

1 Use proper indentation and whitespace to make the code easy to read and understand. 
2 Use uppercase for SQL keywords and lowercase for identifiers (such as table and column names). 
3 Use meaningful and descriptive names for tables, columns, and indexes. --- Consider column naming standard e.g. spaces -> . In R. Learn more from SD and JBe. 
4 Use table aliases to make queries more readable and avoid ambiguity. 
5 Use single quotes for string literals and avoid using double quotes. 
6 Use explicit JOIN syntax instead of the older, implicit JOIN syntax. 
7 Use INNER JOIN instead of WHERE clause to join tables. 
8 Use subqueries sparingly and only when necessary, as they can slow down the performance of a query. 
9 Use UNION instead of UNION ALL when possible, as UNION eliminates duplicate rows. 
10 Use indexes wisely to improve query performance. 
11 Use transactions to ensure data integrity. -- ??? Chat with DW 
12 Always use a WHERE clause in your DELETE and UPDATE statements to avoid accidentally modifying or deleting large amounts of data. 
13 Use views to simplify complex queries and improve security. -- ??? Chat with DW 
14 Keep the code as simple as possible and avoid using unnecessary features or constructs. 
15 Test the code and performance of the queries to ensure that they are working correctly and efficiently. 

