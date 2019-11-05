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

XML:
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

HTML:
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

HTML + JS [+ CSS]
```js
import { LightningElement, track } from 'lwc';
export default class HelloWorld extends LightningElement {
    @track greeting = 'Soma';
    changeHandler(event) {
        this.greeting = event.target.value;
    }
}
```