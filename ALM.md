# Application Lifecycle Management

### Developement Modeles
- Change Set Development
- Org Development
- Package Development

### Application Liecycle Management
1. Plan Release
2. Develop
3. Test
4. Build Release
5. Test Release
6. Release

### Release Categories
- Patch: Bug fixes
- Minor: Limited changes
- Major: Changes with dependencies

### Environments
- Sandbox - Copy of the production org
- Scratch Org - Empty Org

### Change Set Development Model
1. Developer Sandboxes: Isolated
2. Create Outbound Change Set
3. Developer Pro Sandbox
4. Deployment to Production Org: Validation

### Org Development
- Source Control Repository
- Salesforce DX Project
    - Config file: `sfdx-project.json`
    - Deployment Artifact: `.zip`
- Salesforce CLI

### Package Development Model
- Salesforce DX Package
- Skratch Orgs - declarative development
- Project - Scratch Org Sync
- Commit to VCS repo

### Scratch Orgs: CI and Testing
1. Push to Scratch Org for testing (No pull from there)
2. Countinious Integration: Automating consistent test runs for changes
3. Continious Delivery: Package Versions installation in prodoctuon org

Sandbox = representation of the production org