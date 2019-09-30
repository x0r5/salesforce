# Visualforce

Features
- Visualforce standard controller

Example
```Java
<apex:page standardController="Contact" >
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


## Ligthning Web Components
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