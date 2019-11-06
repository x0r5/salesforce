# Visualforce

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


# Ligthning Web Components

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