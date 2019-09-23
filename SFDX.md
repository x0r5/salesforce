# Salesforce CLI
Visual Studio Code: Salesforce Extension Pack

#### Good to know:
Scratch orgs have change tracking. You should use a scratch org to push and pull changes to an from. After it is working as it should, you can deploy your changes with `mdapi:deploy`.

### SFDX In Visual Studio Code
> **Ctrl + Shift + P**
>
> Not All SFDX commands work with Visual Studio sometimes you have to use the terminal `sfdx force:<command>`
>
> However here are some useful commands that are implemented in VS Code:
- SFDX: Create Project
- SFDX: Authorize an org
- SFDX:Execute SOQL Query with Currently Selected Text
- SFDX: Create a Default Scratch Org
- SFDX: Open Default Org
- SFDX: Pull Source from Default Scratch Org

### Config files
- sfdx-project.json
    - Dev org specifications, like url:
        - `"sfdcLoginUrl": "https://<mydomain>.my.salesforce.com",`
- config/project-scratch-def.json
    - Scratch org configurations
- .sfdx/sfdx-config.json
    - sfdx default username
        - `"defaultusername": "test-flmrj2vuvhoc@example.com"`


### Common Workflow !!!
1. Create a project
    - `sfdx:force:project:create -n <projectName>`
2. Authenticate a **Dev Org**
    - `sfdx force:auth:web:login --setalias <alias Dev Org> --instanceurl <domain>.my.salesforce.com --setdefaultusername`
3. Create a **Scratch Org**
    - `sfdx force:org:create -f config/project-scratch-def.json -a myUserAlias`
4. Open the **Scratch Org**
    - `sfdx force:org:open`
5. Pull the changes made on the **Scratch Org**
    - include `**profiles` to `.forceignore` (dont pull not existing profiles)
    - `sfdx force:source:pull`
6. Create changes locally
    - `sfdx force:apex:class:create -n <name>`
7. Push the changes backc to the **Scratch Org**
    - `sfdx force:source:push`
8. Convert source to MDAPI format
    - `sfdx force:source:convert -d mdapiout`
9. Deploy to the **Dev Org** with MDAPI
    - `sfdx force:mdapi:deploy -d mdapiout --wait 100 -u <alias Dev Org>`

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

### Debugging
Use: [Apex Interactive Debugger](https://developer.salesforce.com/tools/vscode/articles/apex/interactive-debugger)

1. Add the DebugApex feature to the scratch org definition files for all the types of scratch orgs that you plan to debug:
`"features": "DebugApex"`


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
