# Data_Comparison_Procedures

Data Comparison Procedures
This repository contains SQL stored procedures for comparing data between two tables based on specified columns.

Overview
The provided SQL stored procedure QA_CompareData compares data between two tables (@tableName1 and @tableName2) based on the specified columns (@productidColumn, @sitecolumn, @priceColumn, @zipColumn). It retrieves records where the values in the specified columns do not match between the two tables.

Stored Procedure
_**CREATE PROCEDURE QA_CompareData
(
    @tableName1 NVARCHAR(255),
    @tableName2 NVARCHAR(255),
    @productidColumn NVARCHAR(255),
    @sitecolumn NVARCHAR(255),
    @priceColumn NVARCHAR(255),
    @zipColumn NVARCHAR(255)
)
AS
BEGIN
    DECLARE @sql NVARCHAR(MAX);
    SET @zipColumn = REPLACE(@zipColumn, ' ', '');
    SET @zipColumn = CAST(@zipColumn AS NVARCHAR(255));
 
    SET @sql = N'
 
    SELECT TOP 500 *
    FROM ' + QUOTENAME(@tableName1) + ' AS a
    INNER JOIN ' + QUOTENAME(@tableName2) + ' AS b
    ON a.' + QUOTENAME(@productidColumn) + ' = b.' + QUOTENAME(@productidColumn) + '
    AND REPLACE(a.zip, '' '', '''') = REPLACE(b.zip, '' '', '''') 
    AND a.' + QUOTENAME(@sitecolumn) + ' = b.' + QUOTENAME(@sitecolumn) + '
    WHERE a.' + QUOTENAME(@priceColumn) + ' <> b.' + QUOTENAME(@priceColumn) + '
    AND REPLACE(a.zip, '' '', '''') = @zipColumn';
 
    EXEC sp_executesql @sql, N'@zipColumn NVARCHAR(255)', @zipColumn;
END;**
_

Description
This stored procedure allows for flexible comparison of data between two tables, facilitating quality assurance checks or data validation processes. By specifying the table names and relevant columns, users can identify discrepancies in data such as product prices, site IDs, or ZIP codes.

How to Use
Clone this repository to your local machine.
Execute the provided SQL stored procedure QA_CompareData in your SQL Server environment.
Provide the required parameters:
@tableName1: Name of the first table to compare.
@tableName2: Name of the second table to compare.
@productidColumn: Column name representing the product ID.
@sitecolumn: Column name representing the site or location.
@priceColumn: Column name representing the price.
@zipColumn: Column name representing the ZIP code.
Analyze the output to identify discrepancies in the specified columns between the two tables.
