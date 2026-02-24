# Data Operations (SOQL, CRUD, Import/Export)

## Run SOQL Query
```bash
sf data query --query "{{soqlQuery}}" --target-org {{org}}
```

## Run SOQL Query (return as CSV)
```bash
sf data query --query "{{soqlQuery}}" --target-org {{org}} --result-format csv
```

## Get a Single Record
```bash
sf data get record --sobject {{objectApiName}} --record-id {{recordId}} --target-org {{org}}
```

## Create a Record
```bash
sf data create record --sobject {{objectApiName}} --values "{{field1}}='{{value1}}' {{field2}}='{{value2}}'" --target-org {{org}}
```

## Update a Record
```bash
sf data update record --sobject {{objectApiName}} --record-id {{recordId}} --values "{{field}}='{{value}}'" --target-org {{org}}
```

## Delete a Record
Confirm with user first. This is a destructive action.
```bash
sf data delete record --sobject {{objectApiName}} --record-id {{recordId}} --target-org {{org}}
```

## Bulk Data Export
```bash
sf data export bulk --query "{{soqlQuery}}" --target-org {{org}} --output-file {{filename}}.csv
```

## Bulk Data Import (Upsert)
```bash
sf data upsert bulk --sobject {{objectApiName}} --file {{csvFile}} --external-id {{externalIdField}} --target-org {{org}}
```

## Bulk Data Import (Insert)
```bash
sf data import bulk --sobject {{objectApiName}} --file {{csvFile}} --target-org {{org}}
```

## Count Records
```bash
sf data query --query "SELECT COUNT() FROM {{objectApiName}}" --target-org {{org}}
```
