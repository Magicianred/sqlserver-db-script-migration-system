# SQLSERVER-DB-SCRIPT-MIGRATION-SYSTEM

An implementation of [SimpleDbScriptMigrationSystem](https://github.com/Magicianred/SimpleDbScriptMigrationSystem) in SqlServer  

## Instructions
1. Run the script 000_InitialScript.sql (to create table for initialize the system)  
2. For each script you write use this header and this footer (like in the *001_ScriptName.sql* script example)

### Header
```sql
-------------------- SCRIPT TO CHECK OF DbScriptMigrationSystem -------------------------------
DECLARE @MigrationName AS VARCHAR(1000) = '001_ScriptName'

IF EXISTS(SELECT MigrationId FROM [DbScriptMigration] WHERE MigrationName = @MigrationName)
BEGIN 
    raiserror('MIGRATION ALREADY RUNNED ON THIS DB!!! STOP EXECUTION SCRIPT', 11, 0)
    SET NOEXEC ON
END

INSERT INTO [DbScriptMigration]
    (MigrationId, MigrationName, MigrationDate)
    VALUES
    (NEWID(), @MigrationName, GETDATE())
GO
PRINT 'Insert record into [DbScriptMigration]!'
-------------------- END SCRIPT TO CHECK OF DbScriptMigrationSystem ---------------------------
```
You have to change the value of the variable *@MigrationName* with your sql script name.  
The *SET NOEXEC ON* instruction block the execution of the sql statements  

### Footer
```sql
---------------- FOOTER OF DbScriptMigrationSystem : REMEMBER TO INSERT -----------------------
SET NOEXEC OFF
```

This instruction resume the normale execution of the sql statements