# User Engagement (Home Pages, Guidance, Notifications)

## Lightning Home Pages

### List Home Page FlexiPages
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel, Type FROM FlexiPage WHERE Type = 'HomePage' ORDER BY MasterLabel" --target-org {{org}} --use-tooling-api
```

### Retrieve Home Page
```bash
sf project retrieve start --metadata FlexiPage:{{homePageDeveloperName}} --target-org {{org}}
```

### Open Lightning App Builder
```bash
sf org open --target-org {{org}} --path /lightning/setup/FlexiPageList/home
```

## In-App Guidance (Prompts)

### Open In-App Guidance Setup
```bash
sf org open --target-org {{org}} --path /lightning/setup/Prompts/home
```

### Retrieve Prompts
```bash
sf project retrieve start --metadata Prompt --target-org {{org}}
```

## Custom Help Text & Descriptions

### View Field Help Text
```bash
sf data query --query "SELECT QualifiedApiName, Label, InlineHelpText FROM FieldDefinition WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}' AND InlineHelpText != null" --target-org {{org}} --use-tooling-api
```

## Path Settings (Sales Path / Kanban)

### List Path Settings
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel, IsActive FROM PathAssistant ORDER BY MasterLabel" --target-org {{org}} --use-tooling-api
```

### Open Path Settings
```bash
sf org open --target-org {{org}} --path /lightning/setup/PathAssistantSetup/home
```

## Custom Notifications

### List Custom Notification Types
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel FROM CustomNotificationType ORDER BY MasterLabel" --target-org {{org}}
```

### Retrieve Custom Notification Type
```bash
sf project retrieve start --metadata CustomNotificationType:{{notifTypeName}} --target-org {{org}}
```

## Utility Bar
Configured per Lightning App. Retrieve the app metadata:
```bash
sf project retrieve start --metadata CustomApplication:{{appName}} --target-org {{org}}
```
Look for `<utilityBar>` section in the app XML.

## Notes
- Home pages are assigned per app + profile combination
- In-App Guidance types: Floating prompt, Docked prompt, Targeted prompt
- Path settings guide users through stages on a record (e.g., Opportunity stages)
