> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/understanding-log-sources-and-investigating-with-splunk/core-spl-commands/question.md).

# Question

#### Question 1

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through an SPL search against all data the account name with the highest amount of Kerberos authentication ticket requests. Enter it as your answer.

```
index="main" sourcetype="WinEventLog:Security" EventCode=4768 | stats count by Account_Name | sort - count | head 1
```

* **`EventCode=4768`**: Event ID 4768 represents Kerberos TGT ticket requests.
* **`stats count by Account_Name`**: Counts the number of events for each account, grouped by the `Account_Name` field.
* **`sort - count`**: Sorts the results in descending order based on the number of requests.
* **`head 1`**: Returns only the first result (the account with the highest number of requests).

***

#### Question 2

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through an SPL search against all 4624 events the count of distinct computers accessed by the account name SYSTEM. Enter it as your answer.

```
index="main" sourcetype="WinEventLog:Security" EventCode=4624 Account_Name="SYSTEM" | stats dc(ComputerName) as Distinct_Computer_Count
```

* **`EventCode=4624`**: Filters only successful logon events (4624).&#x20;
* **`Account_Name="SYSTEM"`**: Filters only events where the account name is `SYSTEM`.&#x20;
* **`stats dc(ComputerName) as Distinct_Computer_Count`**:

`dc(ComputerName)`: Counts the distinct values ​​of the `ComputerName` field, which represents the computers accessed.

`as Distinct_Computer_Count`: Renames the output column to something more readable.

***

#### Question 3

Navigate to http\://\[Target IP]:8000, open the "Search & Reporting" application, and find through an SPL search against all 4624 events the account name that made the most login attempts within a span of 10 minutes. Enter it as your answer.

```
index="main" sourcetype="WinEventLog:Security" EventCode=4624 | stats range(_time) as time_range count by Account_Name | where time_range <= 600 | sort - count | head 1
```

* **`EventCode=4624`**: Filters only successful login events.
* **`stats range(_time) as time_range count by Account_Name`**:
* Calculates the maximum time difference (`range(_time)`) in seconds for each `Account_Name`.
* Counts the number of events (`count`) per account.
* **`where time_range <= 600`**: Filters accounts whose time range (`time_range`) is 10 minutes (600 seconds) or less.
* **`sort - count`**: Sorts accounts by the highest number of attempts.
* **`head 1`**: Returns only the account with the highest number of logins.
