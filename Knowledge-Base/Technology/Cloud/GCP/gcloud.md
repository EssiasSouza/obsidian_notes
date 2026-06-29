Source: #source/internet_resources  
Project: #project/devops 
Areas: #area/work
Subject: #subject/cloud
Type: #type/learning
Learning priority: #priority/P2 
Status: #status/to_learning 
Related: [[GCP - Google Cloud Platform]]

---
### Issues

```
Get-ChildItem : Access to the path 'C:\Program Files (x86)\Google\Cloud SDK\google-cloud-sdk\platform\bundledpython' is denied.
At line:1 char:1
+ Get-ChildItem "C:\Program Files (x86)\Google\Cloud SDK\google-cloud-s ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (C:\Program File...m\bundledpython:String) [Get-ChildItem], UnauthorizedAccessException
    + FullyQualifiedErrorId : DirUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetChildItemCommand
```

Run PowerShell as admin and run:

```
icacls "C:\Program Files (x86)\Google\Cloud SDK\google-cloud-sdk\platform\bundledpython"
```
---

```
Could not find platform independent libraries <prefix>
Fatal Python error: Failed to import encodings module
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'
```

Reinstall Google Cloud SDK

[Quickstart: Install the Google Cloud CLI  |  Google Cloud SDK  |  Google Cloud Documentation](https://docs.cloud.google.com/sdk/docs/install-sdk)