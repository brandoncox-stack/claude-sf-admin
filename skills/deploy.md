# Metadata Deploy & Retrieve

## Retrieve Metadata by Type
```bash
sf project retrieve start --metadata {{MetadataType}}:{{ComponentName}} --target-org {{org}}
```

## Retrieve All Metadata of a Type
```bash
sf project retrieve start --metadata {{MetadataType}} --target-org {{org}}
```

## Deploy Source Directory
```bash
sf project deploy start --source-dir {{path}} --target-org {{org}}
```

## Deploy with Validation Only (checkonly)
```bash
sf project deploy start --source-dir {{path}} --target-org {{org}} --dry-run
```

## Deploy Specific Metadata
```bash
sf project deploy start --metadata {{MetadataType}}:{{ComponentName}} --target-org {{org}}
```

## Check Deploy Status
```bash
sf project deploy report --job-id {{jobId}} --target-org {{org}}
```

## Quick Deploy (after successful validation)
```bash
sf project deploy quick --job-id {{jobId}} --target-org {{org}}
```

## Retrieve Full Project
```bash
sf project retrieve start --target-org {{org}}
```

## Common Metadata Types
- ApexClass, ApexTrigger, ApexPage, ApexComponent
- CustomObject, CustomField, Layout, RecordType
- Flow, FlowDefinition
- PermissionSet, Profile
- LightningComponentBundle
- CustomTab, CustomApplication
