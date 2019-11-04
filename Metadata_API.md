# Apex Metadata API

## Metadata Namespace
- metadata types supported:
    -  page layouts
    - records of custom metadata types
- operations supported:
    - read
    - create
    - update

Metadata.Operations.retrieve()


### Example Code
```java
public class UpdatePageLayout {
    // Add custom field to page layout
    
    public Metadata.Layout buildLayout() {
        
        // Retrieve Account layout and section 
        List<Metadata.Metadata> layouts = 
            Metadata.Operations.retrieve(Metadata.MetadataType.Layout, 
            new List<String> {'Account-Account Layout'});
        Metadata.Layout layoutMd = (Metadata.Layout) layouts.get(0);
        Metadata.LayoutSection layoutSectionToEdit = null;
        List<Metadata.LayoutSection> layoutSections = layoutMd.layoutSections;
        for (Metadata.LayoutSection section : layoutSections) {
            
            if (section.label == 'Account Information') {
                layoutSectionToEdit = section;
                break;
            }
        }
        
        // Add the field under Account info section in the left column
        List<Metadata.LayoutColumn> layoutColumns = layoutSectionToEdit.layoutColumns;     
        List<Metadata.LayoutItem> layoutItems = layoutColumns.get(0).layoutItems;
        
        // Create a new layout item for the custom field
        Metadata.LayoutItem item = new Metadata.LayoutItem();
        item.behavior = Metadata.UiBehavior.Edit;
        item.field = 'AMAPI__Apex_MD_API_sample_field__c';
        layoutItems.add(item);
        
        return layoutMd;
    }
}
```

#### Callback Class (async transfer)
```java
public class PostInstallCallback implements Metadata.DeployCallback {
  
    public void handleResult(Metadata.DeployResult result,
        Metadata.DeployCallbackContext context) {
        
        if (result.status == Metadata.DeployStatus.Succeeded) {
            // Deployment was successful, take appropriate action.
            System.debug('Deployment Succeeded!');
        } else {
            // Deployment wasn’t successful, take appropriate action.
	    System.debug('Deployment Failed!');
        }
    }
}
```