# Salesforce CLI
Visual Studio Code: Salesforce Extension Pack

### SFDX
> **Ctrl + Shift + P**
>
> SFDX: Create Project

*project-scratch-def.json*: 

### Authenticate to Your Playground
1. **Ctrl + Shift + P**
2. SFDX: Authorize an org


### Other SFDX commands
> Use with **Ctrl + Shift + P**
- SFDX:Execute SOQL Query with Currently Selected Text
- SFDX: Create a Default Scratch Org
- SFDX: Open Default Org
- SFDX: Pull Source from Default Scratch Org


### Deploy
1. Right click on the right the component you want to deploy
2. Choose **SFDX: Deploy Source to Org**


### Debugging
Use: [Apex Interactive Debugger](https://developer.salesforce.com/tools/vscode/articles/apex/interactive-debugger)

1. Add the DebugApex feature to the scratch org definition files for all the types of scratch orgs that you plan to debug:
**"features": "DebugApex"**


### Creating a new project
- `sfdx force:project:create -n [name]`
- `sfdx force:apex:class:create -n [ClassName] -d force-app/main/[location]`
- `sfdx force:org:create -f config/project-scratch-def.json -a GeoTestOrg`
- sfdx `force:org:open -u GeoTestOrg`


### SFDX Manage Orgs
- `sfdx force:org:list`
- `sfdx force:org:display`
- `sfdx force:org:open -u`
- `sfdx force:org:delete -u [user]`
- `sfdx force:org:create --definitionfile my-org-def.json --setdefaultusername --setalias my-scratch-org`
- `sfdx force:source:push`


### SFDX Pull from Dev Org
- `sfdx force:mdapi:retrieve -s -r ./mdapipkg -u <username> -p <package name>`
- `sfdx force:mdapi:retrieve:report`
- `unzip`
