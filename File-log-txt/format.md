###Description
This is an standard MSI Windows Installer log


###Example Line
`MSI (c) (6C:C8) [17:03:42:359]: Client-side and UI is none or basic: Running entire install on the server.`


###Layout
|Field|Type|Length|Delimiter(s)|Long Description|
|:---|:---|:---|:---|:--|
|Application notation|String|N/A|{Space}|Typically MSI on Windows
|Action source|String|3|(c/s)|c = client, s = server
|Process and Thread ID|Hex:Hex|7|(processID:threadID)|Only the last 2 Hexadecimal digits of each are shown.
|Timestamp|Time|15|[hh:mm:ss.fff]:|Timestamp only of entry with milliseconds. Note the trailing :
|Message|String|N/A|`CRLF` until MSI for next entry|This can be a multi-line message.  Next record is indicated by line beginning with MSI


###References
- [How to Interpret Windows Installer Logs](http://blogs.technet.com/b/richard_macdonald/archive/2007/04/02/how-to-interpret-windows-installer-logs.aspx)


###Additional Information



