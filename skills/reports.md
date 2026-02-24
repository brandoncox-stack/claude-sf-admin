# Reports & Dashboards

## List Reports
```bash
sf data query --query "SELECT Id, Name, DeveloperName, FolderName FROM Report ORDER BY Name" --target-org {{org}}
```

## List Reports in a Folder
```bash
sf data query --query "SELECT Id, Name, DeveloperName FROM Report WHERE FolderName = '{{folderName}}' ORDER BY Name" --target-org {{org}}
```

## Run a Report (get results)
```bash
sf data query --query "SELECT Id FROM Report WHERE DeveloperName = '{{reportDevName}}'" --target-org {{org}}
# Then use the API:
sf api request rest "/analytics/reports/{{reportId}}" --method GET --target-org {{org}}
```

## List Dashboards
```bash
sf data query --query "SELECT Id, Title, DeveloperName, FolderName FROM Dashboard ORDER BY Title" --target-org {{org}}
```

## Retrieve Report Metadata
```bash
sf project retrieve start --metadata Report:{{reportFolder}}/{{reportName}} --target-org {{org}}
```

## Retrieve Dashboard Metadata
```bash
sf project retrieve start --metadata Dashboard:{{dashFolder}}/{{dashName}} --target-org {{org}}
```
