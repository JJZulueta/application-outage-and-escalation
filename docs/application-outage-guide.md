# Application outage support guide

## 1. Establish scope and impact

Record the exact application, URL, error, time and timezone, affected users and devices, business task, urgency, and available approved workarounds. Distinguish tested facts from user reports and assumptions. Assign priority using the organisation's impact and urgency criteria.

## 2. Gather evidence

Use non-disruptive checks appropriate to your access. In this local lab:

```powershell
[System.Net.Dns]::GetHostAddresses("staffportal.test") | Select-Object IPAddressToString
curl.exe --noproxy "*" --connect-timeout 5 -I http://127.0.0.1:8000
Get-NetTCPConnection -LocalPort 8000 -State Listen
```

Capture errors as well as output. Correct hostname resolution does not prove the application is healthy. A failed HTTP connection does not by itself establish a particular process or firewall as the cause. Compare the checks with the known baseline and console evidence.

## 3. Prepare a useful escalation

Include impact, scope, endpoint, exact symptom, checks and results, changes already made, evidence, uncertainties, and the requested next action. Route to the appropriate team using the organisation's procedures. Keep users informed using confirmed facts and a clear next-update commitment.

## 4. Restore within the correct role and permissions

In this simulation only, the operator starts the existing training server:

```powershell
python -m http.server 8000 --bind 127.0.0.1 --directory C:\ITSupportLab\Portal
```

Keep the server console running. In production, follow an authorised recovery runbook and coordinate changes with the application owner.

## 5. Validate and close

In a second PowerShell window:

```powershell
Get-NetTCPConnection -LocalPort 8000 -State Listen
curl.exe --noproxy "*" --connect-timeout 5 -I http://staffportal.test:8000
```

Confirm the expected listener and HTTP response, then test the browser page. Real applications may also require login, transaction, dependency, and affected-user checks. Record recovery actions, results, remaining issues, and the recovery update before closure. Escalate again if symptoms persist or recur.
