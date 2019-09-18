Navigation: [Apex](Apex.md) | [SFDX](SFDX.md) | [ALM](ALM.md)
# Apex


### Apex Triggers
[Reference](https://developer.salesforce.com/docs/atlas.en-us.220.0.apexcode.meta/apexcode/apex_triggers.htm)
- Usage:
```Apex
trigger TriggerName on ObjectName (trigger_events) {
   //code_block
}
```
- Events:
    - `before insert`
    - `before update`
    - `before delete`
    - `after insert`
    - `after update`
    - `after delete`
    - `after undelete`

- Examples:
```Java
trigger HelloWorldTrigger on Account (before insert, after insert) {
    for(Account a : Trigger.New) {
        a.Description = 'New description';
    }   
    if(Trigger.isBefore){
        //todo
    }
}
```

```Java
trigger AddRelatedRecord on Account(after insert, after update) {
    List<Opportunity> oppList = new List<Opportunity>();
    
    // Get the related opportunities for the accounts in this trigger
    Map<Id,Account> acctsWithOpps = new Map<Id,Account>(
        [SELECT Id,(SELECT Id FROM Opportunities) FROM Account WHERE Id IN :Trigger.New]);
    
    // Add an opportunity for each account if it doesn't already have one.
    // Iterate through each account.
    for(Account a : Trigger.New) {
        System.debug('acctsWithOpps.get(a.Id).Opportunities.size()=' + acctsWithOpps.get(a.Id).Opportunities.size());
        // Check if the account already has a related opportunity.
        if (acctsWithOpps.get(a.Id).Opportunities.size() == 0) {
            // If it doesn't, add a default opportunity
            oppList.add(new Opportunity(Name=a.Name + ' Opportunity',
                                       StageName='Prospecting',
                                       CloseDate=System.today().addMonths(1),
                                       AccountId=a.Id));
        }           
    }
    if (oppList.size() > 0) {
        insert oppList;
    }
}
```

- Note:
>The system saves the records that fired the before trigger after the trigger finishes execution. You can modify the records in the trigger without explicitly calling a DML insert or update operation. If you perform DML statements on those records, you get an error.