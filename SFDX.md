# Salesforce CLI
Salesforce Developer Console (SFDX) and Visual Studio Code Salesforce Extension Pack

Contents:
- [Config Files](#config_files)
- [Project Structure](#project_structure)
- [Usernames and Orgs](#usernames_orgs)
- [Common Workflow](#common_workflow)
- [Creating Project or Parts](#creating)
- [Move Data Between Orgs](#move_data)

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

### Config Files<a name="config_files"></a>
- sfdx-project.json
    - Dev org specifications, like url:
        - `"sfdcLoginUrl": "https://<mydomain>.my.salesforce.com",`
```json
{ 
"packageDirectories" : [ 
    { "path": "force-app", "default": true}, 
    { "path" : "unpackaged" }, 
    { "path" : "utils" } 
  ],
"namespace": "", 
"sfdcLoginUrl" : "https://login.salesforce.com", 
"sourceApiVersion": "44.0"
}
```
- config/project-scratch-def.json
    - Scratch org configurations
```json
{
  "orgName": "Acme",
  "country": "US",
  "edition": "Enterprise",
  "hasSampleData": "true",
  "features": ["MultiCurrency", "AuthorApex"],
  "orgPreferences": {
    "enabled": ["S1DesktopEnabled", "ChatterEnabled"],
    "disabled": ["IsNameSuffixEnabled"]
  }
}
```
- .sfdx/sfdx-config.json
    - sfdx default username for scratch org
        - `"defaultusername": "test-flmrj2vuvhoc@example.com"`

### SFDX Project Structure and Source Format <a name="project_structure"></a>
**Metadata** format is large and uncontrollable -> Break it down to **source format**
Subdirectory tree system
- Static resources
    - `/main/default/staticresources`
    - .resource extension is used ()
    - `.resource-meta.xml`
- File Extensions
    - converting to Source Format creates <filename>-meta.xml files
- Custom Objects
    - `/main/default/objects`

### Usernames and Orgs <a name="usernames_orgs"></a>
- username -> determines an Org
- alias: short version of a username
- Dev HUB: `--setdefaultdevhubusername`
- Scratch Org: `--setdefaultusername`
- `sfdx force:org:list`
    - Default Dev HUB: (D)
    - Default Scratch Org: (U)
- `--targetusername <username or alias>` to deploy not to the default Scratch org
- `--targetdevhubusername <username or alias>` to deploy to a diff. Dev Org
- Set default DevHUB or Scratch Org:
    - `sfdx force:config:set defaultdevhubusername=<username or alias>`
    - `sfdx force:config:set defaultusername=<username or alias of scratch org>`
- Set an  alias:
    - `sfdx force:alias:set <myalias>=<username>`
- Remove an alias:
    - `sfdx force:alias:set <myalias>=`
    



### Common Workflow !!! <a name="common_workflow"></a>
1. Create a project
    - `sfdx force:project:create -n <projectName>`
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

### Creating a new project or class <a name="creating"></a>
- `force:project:create -n <projectname> --template standard`
    - template standard is better for VS Code
- `sfdx force:apex:class:create -n <ClassName> -d force-app/main/<location>`

### Install Required Packages
1. `sfdx force:package:install -i 04t36000000i5UM --wait 100`

### Import / Export Data Between Orgs <a name="move_data"></a>
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

### Package Create and Install
1. `sfdx force:package:version:create --definitionfile config/project-scratch-def.json -p [package alias / id] -k [key] --wait 100 -v [target DEV HUB username] -b [branch name (for easier identification)]`
2. `sfdx force:package:version:list -v [DEV HUB username] -o "CreatedDate"` Check package ID in list
3. `sfdx force:package:install --wait 100 --publishwait 100 --package [package ID] -k [key] -r -u [where to install org alias]`





### SFDX Manage Orgs
- `sfdx force:org:list`
- `sfdx force:org:display`
- `sfdx force:org:open -u`
- `sfdx force:org:delete -u [user]`
- `sfdx force:org:create --definitionfile my-org-def.json --setdefaultusername --setalias my-scratch-org`
- `sfdx force:source:push`


### SFDX Retrive from Dev Org
- With Package Name
    - `sfdx force:mdapi:retrieve -s -r ./mdapipkg -u <username> -p <package name>`
        - username or alias of the target org
        - -s: retriving a single package
- With `package.xml`
    - `sfdx force:mdapi:retrieve -r ./mdapipkg -u <username> -k ./package.xml`
        - package.xml: unpackaged manifest of components to retrive
- Convert Metadata Source to Source Format
    - `sfdx force:mdapi:convert --rootdir mdapi_project --outputdir tmp_convert`
- `sfdx force:mdapi:retrieve:report`
- `unzip`
