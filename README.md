# SQLSERVER-DB-SCRIPT-MIGRATION-SYSTEM

An implementation of [SimpleDbScriptMigrationSystem](https://github.com/Magicianred/SimpleDbScriptMigrationSystem) in SqlServer  

## Instructions
1. Run the script 000_InitialScript.sql (to create table for initialize the system)  
2. For each script you write use this header and this footer (like in the *001_ScriptName.sql* script example)

### Header
```sql
-------------------- SCRIPT TO CHECK OF DbScriptMigrationSystem -------------------------------
DECLARE @MigrationName AS VARCHAR(1000) = '002_ScriptName'

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
You have to change the value of the variable labelled *@MigrationName* with your sql script name.  
The *SET NOEXEC ON* instruction block the execution of the sql statements  

### CHECK PREREQUISITES (one or more, after HEADER)

```sql
-------------------- SCRIPT TO CHECK PREREQUISITES OF DbScriptMigrationSystem -------------------------------
DECLARE @PrerequisiteMigrationName AS VARCHAR(1000) = '001_ScriptName'
IF NOT EXISTS(SELECT MigrationId FROM [DbScriptMigration] WHERE MigrationName = @PrerequisiteMigrationName)
BEGIN 
    raiserror('YOU HAVET TO RUN SCRIPT '+ @PrerequisiteMigrationName +' ON THIS DB!!! STOP EXECUTION SCRIPT', 11, 0)
    SET NOEXEC ON
END

--- one more prerequisite
SET @PrerequisiteMigrationName = 'XXX_ScriptName'
IF NOT EXISTS(SELECT MigrationId FROM [DbScriptMigration] WHERE MigrationName = @PrerequisiteMigrationName)
BEGIN 
    raiserror('YOU HAVET TO RUN SCRIPT '+ @PrerequisiteMigrationName +' ON THIS DB!!! STOP EXECUTION SCRIPT', 11, 0)
    SET NOEXEC ON
END
-------------------- END SCRIPT TO CHECK PREREQUISITES OF DbScriptMigrationSystem ---------------------------
```
Tu devi cambiare il valore della variabile X con il tuo sql script name che vuoi come prerequisito
You have to change the value of the variable labelled *@PrerequisiteMigrationName* with the sql script name that you want to set as a prerequisite.  
The *SET NOEXEC ON* instruction block the execution of the sql statements  


### Footer
```sql
---------------- FOOTER OF DbScriptMigrationSystem : REMEMBER TO INSERT -----------------------
SET NOEXEC OFF
```

This instruction resume the normale execution of the sql statements