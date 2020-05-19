# Visualforce and Lightning Web Components
- [Visualfroce](#visualforce)
- [Lightning Web Components](#lwc)


## Visualforce <a name="visualforce"></a>
Features
- Visualforce standard controller

Example
```html
<apex:page standardStylesheets="false" standardController="Contact" >
    <apex:form >
        
        <apex:pageBlock title="Edit Contact">
            <apex:pageBlockSection columns="1">
                <apex:inputField value="{!Contact.FirstName}"/>
                <apex:inputField value="{!Contact.LastName}"/>
                <apex:inputField value="{!Contact.Email}"/>
                <apex:inputField value="{!Contact.Birthdate}"/>
            </apex:pageBlockSection>
            <apex:pageBlockButtons >
                <apex:commandButton action="{!save}" value="Save"/>
            </apex:pageBlockButtons>
        </apex:pageBlock>
        
    </apex:form>
</apex:page>
```

## Global variables
$User.FirstName ..
$

## Standard Controllers
<apex:page standardController="Account">
Name: {! Account.Name }


# Ligthning Web Components <a name="lwc"></a>

Lightning Experience contains Lightning Elements
+ app-centric model
+ Out-of-the-Box Component Set
+ Build with: Lightning Web Components model or Aura Components

All Lightning Components have a namespace that is separated from the folder name with a hyphen: 
- app -> c-app
- viewSource -> c-view-source

## XML:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
   <apiVersion>45.0</apiVersion>
   <isExposed>true</isExposed>
   <targets>
       <target>lightning__AppPage</target>
       <target>lightning__RecordPage</target>
       <target>lightning__HomePage</target>
   </targets>
</LightningComponentBundle>
```
Parts:
- apiVersion
- isExposed: T/F
- targets: which types of Lightning Pages the component can be used in Lightning App Builder
- targetConfigs

## HTML:
```html
<template>
    <lightning-card title="HelloWorld" icon-name="custom:custom14">
        <div class="slds-m-around_medium">
            <p>Hello, {greeting}!</p>
            <lightning-input label="Name" value={greeting} onchange={changeHandler}></lightning-input>
        </div>
    </lightning-card>
</template>
```
Parts:
- `<template></template>` tags
-  identifiers in the curly braces `{variable}`

## Java Script:
```js
import { LightningElement, track } from 'lwc';
export default class HelloWorld extends LightningElement {
    @track greeting = 'Soma';
    changeHandler(event) {
        this.greeting = event.target.value;
    }
}
```
Parts:
- `import` statement
- `export default class <className> extends LightningElement{<variables...>}`
- `@track`: required for dynamically changing variables to re-render the page
- `@api`: Marks a property as public for use in your template or other components.
- `@wire`: Gives you a way to get and bind data.

## Wire Service to get Salesforce Data
- wire adapter is needed
- wire decorator for a function or property
```js
import { adapterId } from 'adapter-module';
@wire(adapterId, adapterConfig)
propertyOrFunction;
```

## Component Lifecycle Hooks
- Created: 
- Added to the DOM: `connectedCallback()`
- Rendered in the browser
- Encountering errors
- Removed from the DOM: `disconnectedCallback()`


## Use ``c-tile``
```html
<template>
    <div class="container">
        <template for:each={bikes} for:item="bike">
            <c-tile 
                key={bike.fields.Id.value} 
                product={bike} 
                ontileclick={handleTileClick}>
            </c-tile>
        </template>
    </div>
</template>
```

## Handling events in HTML and JS
> events up, properties down
```html
<template>
    <div class="container">
        <a onclick={tileClick}>
            <div class="title">{product.fields.Name.value}</div>
            <img class="product-img" src={product.fields.Picture_URL__c.value}></img>
        </a>
    </div>
</template>
```
```js
import { LightningElement, api } from 'lwc';
export default class Tile extends LightningElement {
    @api product;
    tileClick() {
        const event = new CustomEvent('tileclick', {
            // detail contains only primitives
            detail: this.product.fields.Id.value
        });
        // Fire the event from c-tile
        this.dispatchEvent(event);
    }
}
```

## Code Snippets
### Wire SF function to get data
```js
const columns = [
    { label: 'Name', fieldName: 'Name', type: 'text' },
    { label: 'Active', fieldName: 'Active__c', type: 'boolean', 
        "cellAttributes": {
        "iconName": { "fieldName": "Active__c_chk" },
        "iconPosition": "left"
    }},
];
export default class FilterCases extends LightningElement{
    @wire(getBlacklist)
        wiredBlacklist( result ) {
            this.wiredBlacklistResult = result;
            if(result.data){
                this.errorBlacklist = undefined;
                console.log(result.data);
                let _data = [];
                for(let row of result.data){
                    _data.push(Object.assign({},row));
                }
                this.blacklist = _data;

            } else if(result.error){
                this.errorBlacklist = result.error;
                this.blacklist = undefined;
                console.log(result.error);
            }
        }

    saveMethod(){
        if(this.inputKeyword){
            addKeyword({keyword: this.inputKeyword, active: this.inputCheckbox, type: this.type, description: this.inputDescription})
            .then(() => {
                this.showSuccessNotification('Successfully created / updated record');
                if(this.type == 'blacklist'){
                    refreshApex(this.wiredBlacklistResult);
                }
                else{
                    refreshApex(this.wiredWhitelistResult);
                }
            }).catch((error) => {
             this.showErrorNotification(error);
            });
            this.openmodel = false;
            window.location.reload();
            
        }else{
            console.log('no keyword');
        }
    }
}
```
```java
@AuraEnabled(cacheable=true)
    public static List<Social_Blacklist__c> getBlacklist(){
        Map<String, Social_Blacklist__c> blacklist_map = Social_Blacklist__c.getAll();
        return blacklist_map.values();

    }
```

```html
<template>
<lightning-card>
    <lightning-button-group>
    <lightning-button label="Create New Rule" title="Create New Rule" onclick={createNew}></lightning-button>
    <lightning-button label="Edit Selected Row" onclick={editRow}></lightning-button>
    <lightning-button label="Run Rules on Open Cases" onclick={runRules}></lightning-button>
</lightning-button-group>
</lightning-card>
<lightning-card>
<lightning-tabset class="tabset">
 <lightning-tab label="Blacklist" onactive={handleActiveBlacklist}>
    <template if:true={blacklist}>
        <lightning-datatable
                class="blacklist"
                key-field="name"
                data={blacklist}
                columns={columns}
                max-row-selection=1>
        </lightning-datatable>
        </template>
        
        
        <template if:true={errorBlacklist}>
        <h1>Error happened</h1>
        {errorBlacklist}
        </template>

    </lightning-tab>
  </lightning-tabset>
</lightning-card>


<template if:true={openmodel}>
    <div class="demo-only" style="height: 640px;">
        <section role="dialog" tabindex="-1" aria-labelledby="modal-heading-01" aria-modal="true" aria-describedby="modal-content-id-1" class="slds-modal slds-fade-in-open">
            <div class="slds-modal__container">
                <header class="slds-modal__header">
                    <button class="slds-button slds-button_icon slds-modal__close slds-button_icon-inverse" title="Close" onclick={closeModal}>
                        <lightning-icon icon-name="utility:close" size="medium">
                        </lightning-icon>
                        <span class="slds-assistive-text">Close</span>
                    </button>
                    <h2 id="modal-heading-01" class="slds-text-heading_medium slds-hyphenate">Új {type} Szabály</h2>
                </header>
                <div class="slds-modal__content slds-p-around_medium" id="modal-content-id-1">
                    
                        <div class="slds-m-around_medium">
                            <template if:true={postidActive}>
                                <lightning-input type="text" label="Description" onchange={handleDescriptionChange} value={inputDescription}></lightning-input>
                            </template>
                            <lightning-input type="text" label="keyword" onchange={handleKeywordChange} value={inputKeyword}></lightning-input>
                            <lightning-input type="checkbox" label="active" onchange={handleCheckboxChange} checked={inputCheckbox}></lightning-input>
                            <lightning-button label="save" onclick={saveMethod}></lightning-button>

                                
                            </div>

                </div>
                <footer class="slds-modal__footer">
                    <lightning-button label="Close" variant="brand" onclick={closeModal}></lightning-button>
                </footer>
            </div>
        </section>
        <div class="slds-backdrop slds-backdrop_open"></div>
    </div>
</template>
```