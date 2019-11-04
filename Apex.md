Navigation: [Apex](Apex.md) | [SFDX](SFDX.md) | [ALM](ALM.md) | [Visualforce](Visualforce.md) | [Lightning Flow](Lightning_Flow.md) | [HOME](README.md)
# Apex

Content:
- [Apex Triggers](#triggers)
- [Asynchronous Apex](#async)
- 


## Apex Triggers <a name='triggers'></a>
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


## Asynchronous Apex <a name='async'></a>
### Future Apex
Must be: `static` and `void`

Example
```Java
public class SMSUtils {
    // Call async from triggers, etc, where callouts are not permitted.
    @future(callout=true)
    public static void sendSMSAsync(String fromNbr, String toNbr, String m) {
        String results = sendSMS(fromNbr, toNbr, m);
        System.debug(results);
    }
    // Call from controllers, etc, for immediate processing
    public static String sendSMS(String fromNbr, String toNbr, String m) {
        // Calling 'send' will result in a callout
        String results = SmsMessage.send(fromNbr, toNbr, m);
        insert new SMS_Log__c(to__c=toNbr, from__c=fromNbr, msg__c=results);
        return results;
    }
}
```
### Batch Apex
Implement `Database.Batchable` interface and methods:
- **start**
    - `Database.QueryLocator` or `Iterable` object is returned
    - max 50 million records with the QueryLocator
- **execute**
    - processing for each batch (200 record)
    - asynchronous execution
    - arguments:
        - `Database.BatchableContext`
        - `List<sObject>`
- **finish**
    - called after all batches finished: post-processing

Skeleton code:
```Java
global class MyBatchClass implements Database.Batchable<sObject> {
    global (Database.QueryLocator | Iterable<sObject>) start(Database.BatchableContext bc) {
        // collect the batches of records or objects to be passed to execute
    }
    global void execute(Database.BatchableContext bc, List<P> records){
        // process each batch of records
    }    
    global void finish(Database.BatchableContext bc){
        // execute any post-processing operations
    }    
}
// execution:
MyBatchClass myBatchObject = new MyBatchClass(); 
Id batchId = Database.executeBatch(myBatchObject, 100); //batch size can be passed also
```


### Queuable Apex
Monitoring: `System.enqueueJob`<br>
Chaining Jobs

Skeleton:
```Java
public class SomeClass implements Queueable { 
    public void execute(QueueableContext context) {
        // awesome code here
    }
}
```
Example:
```Java
public class UpdateParentAccount implements Queueable {
    
    private List<Account> accounts;
    private ID parent;
    
    public UpdateParentAccount(List<Account> records, ID id) {
        this.accounts = records;
        this.parent = id;
    }
    public void execute(QueueableContext context) {
        for (Account account : accounts) {
          account.parentId = parent;
          // perform other processing or callout
        }
        update accounts;
    }
    
}
```

## Apex Properties <a name='properties'></a>
two code blocks:
- The code in a get accessor executes when the property is read.
- The code in a set accessor executes when the property is assigned a new value.
```java
public class BasicProperty {
   public integer prop {
      get { return prop; }
      set { prop = value; }
   }
}
```