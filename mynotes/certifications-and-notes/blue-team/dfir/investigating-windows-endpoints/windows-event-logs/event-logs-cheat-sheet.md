> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/windows-event-logs/event-logs-cheat-sheet.md).

# Event Logs Cheat Sheet

#### Security Event IDs of Interest

<table><thead><tr><th width="106.75259399414062">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>4624</td><td>An account was successfully logged on. (See Logon Type Codes)</td></tr><tr><td>4625</td><td>An account failed to log on.</td></tr><tr><td>4634</td><td>An account was logged off</td></tr><tr><td>4647</td><td>User initiated logoff. (In place of 4634 for Interactive and RemoteInteractive logons)</td></tr><tr><td>4648</td><td>A logon was attempted using explicit credentials. (RunAs)</td></tr><tr><td>4672</td><td>Special privileges assigned to new logon. (Admin login)</td></tr><tr><td>4776</td><td>The domain controller attempted to validate the credentials for an account. (DC)</td></tr><tr><td>4768</td><td>A Kerberos authentication ticket (TGT) was requested.</td></tr><tr><td>4769</td><td>A Kerberos service ticket was requested.</td></tr><tr><td>4771</td><td>Kerberos pre-authentication failed.</td></tr><tr><td>4720</td><td>A user account was created.</td></tr><tr><td>4722</td><td>A user account was enabled.</td></tr><tr><td>4688</td><td>A new process has been created. (If audited; some Windows processes logged by default)</td></tr><tr><td>4698</td><td>A scheduled task was created. (If audited)</td></tr><tr><td>4798</td><td>A user's local group membership was enumerated.</td></tr><tr><td>4799</td><td>A security-enabled local group membership was enumerated.</td></tr><tr><td>5140</td><td>A network share object was accessed.</td></tr><tr><td>5145</td><td>A network share object was checked to see whether client can be granted desired access.</td></tr><tr><td>1102</td><td>The audit log was cleared. (Security)</td></tr></tbody></table>

***

#### Logon Type Codes

<table><thead><tr><th width="100.66162109375">Type</th><th>Description</th></tr></thead><tbody><tr><td>2</td><td>Console</td></tr><tr><td>3</td><td>Network</td></tr><tr><td>4</td><td>Batch (Scheduled Tasks)</td></tr><tr><td>5</td><td>Windows Services</td></tr><tr><td>7</td><td>Screen Lock/Unlock</td></tr><tr><td>8</td><td>Network (Cleartext Logon)</td></tr><tr><td>9</td><td>Alternate Credentials Specified (RunAs)</td></tr><tr><td>10</td><td>Remote Interactive (RDP)</td></tr><tr><td>11</td><td>Cached Credentials (e.g., Offline DC)</td></tr><tr><td>12</td><td>Cached Remote Interactive (RDP, similar to Type 10)</td></tr><tr><td>13</td><td>Cached Unlock (Similar to Type 7)</td></tr></tbody></table>

***

#### System Event IDs of Interest

<table><thead><tr><th width="103.01187133789062">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>7045</td><td>A new service was installed in the system. (4697 in Security)</td></tr><tr><td>7034</td><td>The x service terminated unexpectedly. It has done this y time(s).</td></tr><tr><td>7009</td><td>A timeout was reached (x milliseconds) while waiting for the y service to connect.</td></tr><tr><td>104</td><td>The x log file was cleared. (Will show System, Application, and other logs cleared)</td></tr></tbody></table>

***

#### Application Event IDs of Interest

<table><thead><tr><th width="105.1171875">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>*1000</td><td>Application Error</td></tr><tr><td>*1002</td><td>Application Hang</td></tr></tbody></table>

*\*Remember, third-party software (like Antivirus) can also write to this log!*

***

#### Application (ESENT Provider) Event IDs of Interest

<table><thead><tr><th width="123.10528564453125">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>216</td><td>A database location change was detected.</td></tr><tr><td>325</td><td>The database engine created a new database.</td></tr><tr><td>326</td><td>The database engine attached a database.</td></tr><tr><td>327</td><td>The database engine detached a database.</td></tr></tbody></table>

***

#### Windows-PowerShell Event IDs of Interest

<table><thead><tr><th width="104.08355712890625">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>400</td><td>Engine state is changed from None to Available.</td></tr><tr><td>600</td><td>Provider "x" is Started.</td></tr></tbody></table>

***

#### Microsoft-Windows-PowerShell/Operational Event IDs of Interest

<table><thead><tr><th width="135.05108642578125">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>*4104</td><td>4104, Creating Scriptblock text (1 of 1): (Scriptblock Logging)</td></tr></tbody></table>

*\*Enabled by default in PowerShell v5 and later for scripts identified as potentially malicious, logged as warnings*

***

#### Microsoft-Windows-TaskScheduler/Operational Event IDs of Interest

<table><thead><tr><th width="142.43670654296875">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>106</td><td>The user x registered the Task Scheduler task y. (New Scheduled Task)</td></tr><tr><td>141</td><td>User x deleted Task Scheduler task y.</td></tr><tr><td>100</td><td>Task Scheduler started the x instance of the y task for user z.</td></tr><tr><td>102</td><td>Task Scheduler successfully finished the x instance of the y task for user z.</td></tr></tbody></table>

#### Microsoft-Windows-Windows Defender/Operational Event IDs of Interest

<table><thead><tr><th width="107.2965087890625">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>1116</td><td>The antimalware platform detected malware or other potentially unwanted software.</td></tr><tr><td>1117</td><td>The antimalware platform performed an action to protect your system from malware or other potentially unwanted<br>software</td></tr></tbody></table>

***

#### Microsoft-Windows-TerminalServices-LocalSessionManager/Operational Event IDs of Interest

<table><thead><tr><th width="102.5517578125">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>21</td><td>Remote Desktop Services: Session logon succeeded:</td></tr><tr><td>22</td><td>Remote Desktop Services: Shell start notification received:</td></tr><tr><td>23</td><td>Remote Desktop Services: Session logoff succeeded:</td></tr><tr><td>24</td><td>Remote Desktop Services: Session has been disconnected:</td></tr><tr><td>25</td><td>Remote Desktop Services: Session reconnection succeeded:</td></tr></tbody></table>

***

#### Microsoft-Windows-TerminalServices-RemoteConnectionManager/Operational Event IDs of Interest

<table><thead><tr><th width="104.01370239257812">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>*1149</td><td>Remote Desktop Services: User authentication succeeded:</td></tr><tr><td>261</td><td>Listener RDP-Tcp received a connection</td></tr></tbody></table>

*\*Event ID 1149 indicates successful network authentication, which occurs prior to user authentication, but in newer versions of Windows it has been observed*\
*that this event is only logged when the subsequent user authentication is successful*

***

#### Microsoft-Windows-TerminalServices-RDPClient/Operational Event IDs of Interest

<table><thead><tr><th width="102.9556884765625">Event ID</th><th>Description</th></tr></thead><tbody><tr><td>*1029</td><td>Base64(SHA256(UserName)) is = HASH</td></tr></tbody></table>

*\*Created on the computer INITIATING the connection (i.e., the SOURCE); contains a HASH of the username used*
