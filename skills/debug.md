# Debug Logs & Troubleshooting

## Set Up Debug Log for User
```bash
sf data create record --sobject TraceFlag --values "TracedEntityId='{{userId}}' DebugLevelId='{{debugLevelId}}' LogType='USER_DEBUG' StartDate='{{startDate}}' ExpirationDate='{{expirationDate}}'" --target-org {{org}} --use-tooling-api
```

## List Debug Levels
```bash
sf data query --query "SELECT Id, DeveloperName, MasterLabel FROM DebugLevel" --target-org {{org}} --use-tooling-api
```

## List Recent Debug Logs
```bash
sf apex log list --target-org {{org}}
```

## Get a Debug Log
```bash
sf apex log get --log-id {{logId}} --target-org {{org}}
```

## Get Latest Debug Log
```bash
sf apex log get --number 1 --target-org {{org}}
```

## Tail Logs (real-time)
```bash
sf apex tail log --target-org {{org}}
```

## Delete All Logs
```bash
sf apex log delete --target-org {{org}} --no-prompt
```

## Check Active Trace Flags
```bash
sf data query --query "SELECT Id, TracedEntity.Name, LogType, StartDate, ExpirationDate FROM TraceFlag WHERE ExpirationDate > TODAY" --target-org {{org}} --use-tooling-api
```
