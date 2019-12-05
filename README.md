# zabbix-mssql-2008-2012-powershell
Zabbix template and scripts to monitor MSSQL via process counters and DB discovers via powershell and Zabbix active checks.

This is heavily based on https://github.com/JPangburn314/Zabbix3.4-MSSQL-2008-2016 - So thanks for the work!

These templates and scripts have been tested with MSSQL 2012 and Zabbix 4.2. The template and config are configured for the Zabbix agent to be installed under `C:\zabbix\`. You will need to change the template and `userparameter_mssql.conf` to represent your agent's directory, as well as the setup below.

## Requirements

 - Powershell v3 is required because the scripts use `ConvertTo-Json`.

## Install

1. Import the `Zabbix MSSQL 2008-2016 template.xml` template to the Zabbix UI. Under Configuration --> Templates.

2. In the Zabbix UI, create a Value Mapping for the database state's. Under Administration --> General, go to Value Mapping on the right side. Add a new mapping like this:

```
Name: MS SQL Server database state
```

| Value |	Mapped to |
| ----- | --------- |
| 0	| ONLINE |
1	| RESTORING
2	| RECOVERING
3	| RECOVERY PENDING
4	| SUSPECT
5	| EMERGENCY
6	| OFFLINE
7	| Database Does Not Exist on Server

3. In the Zabbix UI, create a Regular expression for the database discovery. Under Administration --> General, go to Regular expressions on the right side. Add a new expression like this:

```
Name: MSSQL Databases for discovery
Expression type: Result is False
Expression: ^(master|model|msdb|ReportServer|ReportServerTempDB|tempdb)$
```

4. On the server you want to monitor, install the Zabbix agent for Windows.

5. Copy the `zabbix_agentd.conf.d` and `scripts` folders to the server.

6. The `userparameter_mssql.conf` file goes under `c:\zabbix\zabbix_agentd.conf.d` and all the scripts go under `c:\zabbix\scripts`

7. Add the SQL server and assign the template to the host in the Zabbix UX.

8. Restart the Zabbix Proxy and/or Zabbix Agent on the server for the changes to immediately apply.
