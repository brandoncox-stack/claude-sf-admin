# Lightning Experience Customization (Apps, Layouts, Pages, Actions)

## List Custom Applications
```bash
sf data query --query "SELECT Id, DeveloperName, Label, Description FROM CustomApplication ORDER BY Label" --target-org {{org}} --use-tooling-api
```

## List Lightning Apps
```bash
sf data query --query "SELECT DurableId, Label, DeveloperName FROM AppDefinition ORDER BY Label" --target-org {{org}} --use-tooling-api
```

## Retrieve App Metadata
```bash
sf project retrieve start --metadata CustomApplication:{{appDeveloperName}} --target-org {{org}}
```

## List Page Layouts for an Object
```bash
sf data query --query "SELECT Id, Name, EntityDefinitionId FROM Layout WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}' ORDER BY Name" --target-org {{org}} --use-tooling-api
```

## Retrieve Page Layout
```bash
sf project retrieve start --metadata Layout:{{objectApiName}}-{{layoutName}} --target-org {{org}}
```

## List Compact Layouts for an Object
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel FROM CompactLayout WHERE SobjectType = '{{objectApiName}}'" --target-org {{org}} --use-tooling-api
```

## Retrieve Compact Layout
```bash
sf project retrieve start --metadata CompactLayout:{{objectApiName}}.{{compactLayoutName}} --target-org {{org}}
```

## List Lightning Record Pages (FlexiPages)
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel, Type, EntityDefinitionId FROM FlexiPage ORDER BY MasterLabel" --target-org {{org}} --use-tooling-api
```

## Retrieve Lightning Record Page
```bash
sf project retrieve start --metadata FlexiPage:{{flexiPageDeveloperName}} --target-org {{org}}
```

## List Quick Actions (Global)
```bash
sf data query --query "SELECT Id, DeveloperName, Label, Type, SobjectType FROM QuickActionDefinition WHERE SobjectType = null ORDER BY Label" --target-org {{org}} --use-tooling-api
```

## List Quick Actions on an Object
```bash
sf data query --query "SELECT Id, DeveloperName, Label, Type FROM QuickActionDefinition WHERE SobjectType = '{{objectApiName}}' ORDER BY Label" --target-org {{org}} --use-tooling-api
```

## Retrieve Quick Action
```bash
sf project retrieve start --metadata QuickAction:{{objectApiName}}.{{actionName}} --target-org {{org}}
```

## List Custom Buttons/Links on an Object
```bash
sf data query --query "SELECT Id, Name, ContentSource, LinkType FROM WebLink WHERE EntityDefinition.QualifiedApiName = '{{objectApiName}}'" --target-org {{org}} --use-tooling-api
```

## List Custom Tabs
```bash
sf data query --query "SELECT Id, Name, Label, Type, SobjectName FROM CustomTab ORDER BY Label" --target-org {{org}} --use-tooling-api
```

## Retrieve Custom Tab
```bash
sf project retrieve start --metadata CustomTab:{{tabName}} --target-org {{org}}
```

## List List Views for an Object
```bash
sf data query --query "SELECT Id, Name, DeveloperName, SobjectType FROM ListView WHERE SobjectType = '{{objectApiName}}' ORDER BY Name" --target-org {{org}}
```

## Retrieve List View
```bash
sf project retrieve start --metadata ListView:{{objectApiName}}.{{listViewDeveloperName}} --target-org {{org}}
```

## Deploy Layout/Page Changes
```bash
sf project deploy start --source-dir force-app/main/default/layouts --target-org {{org}}
sf project deploy start --source-dir force-app/main/default/flexipages --target-org {{org}}
```

## Open Setup Pages
```bash
# App Manager
sf org open --target-org {{org}} --path /lightning/setup/NavigationMenus/home
# Object Manager
sf org open --target-org {{org}} --path /lightning/setup/ObjectManager/home
# Lightning App Builder
sf org open --target-org {{org}} --path /lightning/setup/FlexiPageList/home
```
