# Package Management

## List Installed Packages
```bash
sf package installed list --target-org {{org}}
```

## Install a Package by Version ID
```bash
sf package install --package {{04tXXXXXXXXXXXXXXX}} --target-org {{org}} --wait 10 --no-prompt
```

## Install a Package by URL
Extract the package ID from the URL and use the command above.

## Check Install Status
```bash
sf package install report --request-id {{requestId}} --target-org {{org}}
```

## Uninstall a Package
Confirm with user first. This is a destructive action.
```bash
sf package uninstall --package {{04tXXXXXXXXXXXXXXX}} --target-org {{org}} --wait 10
```

## List Package Versions (for packages you own)
```bash
sf package version list --target-org {{org}}
```

## Create a Package Version
```bash
sf package version create --package {{packageName}} --path {{path}} --installation-key-bypass --wait 10 --target-dev-hub {{devHub}}
```
