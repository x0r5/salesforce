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

HTML + JS [+ CSS]
```js
// import module elements
import { LightningElement, track } from 'lwc';
// declare class to expose the component
export default class App extends LightningElement {

// add decorator   
    @track 
    ready = false;

// use lifecycle hook
    connectedCallback() {
        setTimeout(() => {
            this.ready = true;
        }, 3000);
    }
}
```