# Salesforce CLI
Visual Studio Code: Salesforce Extension Pack

#### Good to know:
Scratch orgs have change tracking. You should use a scratch org to push and pull changes to an from. After it is working as it should, you can deploy your changes with `mdapi:deploy`.

### SFDX
> **Ctrl + Shift + P**
>
> SFDX: Create Project

*project-scratch-def.json*: 

### Authenticate to Your Playground
1. **Ctrl + Shift + P**
2. SFDX: Authorize an org

### Common Workflow !!!
1. `sfdx:force:project:create -n <projectName>`
2. `sfdx force:auth:web:login --setalias <alias> --instanceurl <domain>.my.salesforce.com --setdefaultusername`
3. `sfdx force:org:create -s -f config/project-scratch-def.json` [create scratch org]
4. `sfdx force:org:open` [opens the scratch org]
5. `sfdx force:source:pull` [pull the changes made on the scratch org]
6. `sfdx force:apex:class:create -n <name>` [create changes locally]
7. `sfdx force:source:push` [push the changes backc to the scratch org]
8. `sfdx force:source:convert -d mdapiout` [convert source to MDAPI format]
9. `sfdx force:mdapi:deploy -d mdapiout --wait 100 -u <alias>` [deploy the main org]
+ include `**profiles` to `.forceignore` (dont pull not existing profiles)

### Install Required Packages
1. `sfdx force:package:install -i 04t36000000i5UM --wait 100`

### Import / Export Data Between Orgs
1. `force:data:soql:query -q "SELECT Waypoint_Name__c FROM Waypoints__c"` [test data soql query]
2. `sfdx force:data:tree:export -q "SELECT Waypoint_Name__c FROM Waypoints__c" -p -d data` [export to JSON the data]
3. `sfdx force:data:tree:import -p data/Waypoint__c-plan.json -u <alias>` [import to main org]

### Executing Anonymus Apex Code
1. create folder `scripts`
2. create `anonymus.apex` file
3. `sfdx force:apex:execute -f scripts/anonymus.apex -u <alias>` [execute on the `alias` org]


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
- `sfdx force:org:open -u GeoTestOrg`


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
