# Correct settings for ODBC driver 
## Background

Reading special characters with Informatica, beyond the German or English ANSI codepage, results in character strings like "Ã, ¨". 

Informatica allows us to configure whether the ANSI interfaces or Unicode interfaces of the ODBC driver are to be used.  
On the technical level of the ODBC-API, this means whether to call function SQLPrepareA (ANSI) or SQLPrepareW (Unicode).  
Other examples would be SQLConnectA/SQLExecDirectA versus SQLConnectW/SQLExecDirectW.

The W-functions are using wchar_t data types. In Microsoft Windows, these are encoded by UTF-16 little-endian (previously, for Windows NT, it was UCS-2). Thus, there should not occur any problems under Windows as long as the Unicode interfaces are used, because each application under Windows should respect this "convention". 

## Prerequisites

Informatica PowerCenter. Windows 2008R2.  Informatica has to be configured to use Unicode interfaces via UTF-8 encoding. Additionally, it is necessary to enforce the ODBC driver to apply non-standard UTF-8 encoding for its W-interfaces and to avoid any further issues for its the A-interfaces as well.

## How to set ODBC settings for special characters

## Step 1

This could be done via ODBC connection string:


```"noformat
AnsiArgEncoding=CP_UTF8;AnsiDataEncoding=CP_UTF8;UnicodeArgEncoding=CP_UTF8;UnicodeDataEncoding=CP_UTF8 
```
Alternatively, the Windows registry can be used to apply this setting on the system level for the Informatica Windows server.  
For an exemplary ODBC DSN "myexaodbcdsn", the following REG file would do the job:


```"noformat
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI\myexaodbcdsn]
"AnsiArgEncoding"="CP_UTF8"
"AnsiDataEncoding"="CP_UTF8"
"UnicodeArgEncoding"="CP_UTF8"
"UnicodeDataEncoding"="CP_UTF8"
```
## Step 2

For testing the correct behavior, a vanilla (plain, ordinary)  table can be used.  
The table was created via JDBC, with EXAplus/JDBC/Java there had not been any Unicode problems.

## Additional References

[Unicode-support-in-exasol](https://community.exasol.com/t5/database-features/unicode-support-in-exasol/ta-p/775) 

[How-to-use-exasol-as-a-linked-server-in-sql-server](https://community.exasol.com/t5/connect-with-exasol/how-to-use-exasol-as-a-linked-server-in-sql-server/ta-p/362) 

