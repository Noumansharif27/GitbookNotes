> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/dfir/investigating-windows-endpoints/windows-event-logs/fundamentals.md).

# Fundamentals

The Windows Event Log is a centralized record of **significant events** on a computer, including system, security, and application activities. It is a critical tool for system administrators to troubleshoot issues, monitor performance, and maintain security. Key Components and Types of LogsWindows categorizes events into different logs and severity levels:Main Log Types

* **Application**: Events logged by software applications installed on the system.
* **Security**: Events related to security activities, such as valid and invalid logon attempts, file deletions, and access to resources.
* **System**: Events logged by core Windows system components and services, such as driver failures during startup.
* **Setup**: Events related to the installation and updates of the Windows operating system and its components.
* **Forwarded Events**: A collection of logs from other computers, typically centralized for easier monitoring.
* **Applications and Services Logs**: More detailed logs for specific applications or services, including Admin, Operational, Analytic, and Debug subtypes.&#x20;

Event Levels (Severity)Events are classified by severity to help prioritize issues:&#x20;

* **Critical**: Indicates a severe problem that requires immediate attention, like an application crash.
* **Error**: Describes a significant issue, such as data loss, but might not need immediate attention.
* **Warning**: Suggests a potential future problem or an unusual situation that administrators should monitor.
* **Information**: Confirms the successful operation of a program, driver, or service.
* **Verbose**: Provides detailed progress or success messages, typically for specific diagnostic purposes.&#x20;

Accessing Windows Event LogsThe primary tool for viewing and managing event logs is the **Event Viewer**, a built-in Microsoft Management Console (MMC) snap-in. You can open the Event Viewer in several ways:

* **Run dialog**: Press the `Windows` key + `R`, type `eventvwr` or `eventvwr.msc`, and press `Enter`.
* **Windows Search**: Type "Event Viewer" in the Start menu search bar and select the application.
* **Computer Management**: Open Computer Management and navigate to **System Tools** > **Event Viewer**.&#x20;

For more advanced management and automation, administrators can use PowerShell cmdlets like `Get-WinEvent` (recommended for modern Windows versions). Common Uses

* **Troubleshooting**: Pinpointing the source of errors, crashes, or unexpected system behavior using specific Event IDs and timestamps.
* **Security Auditing**: Monitoring for unauthorized access attempts, policy changes, and other security-related incidents.
* **Performance Monitoring**: Identifying potential bottlenecks or hardware issues, such as low disk space, through warning events
