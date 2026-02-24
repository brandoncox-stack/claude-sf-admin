# Apex Execution & Testing

## Execute Anonymous Apex
```bash
sf apex run --file {{apexFile}} --target-org {{org}}
```
Or inline:
```bash
echo "{{apexCode}}" | sf apex run --target-org {{org}}
```

## Run All Tests
```bash
sf apex run test --target-org {{org}} --wait 10
```

## Run Specific Test Class
```bash
sf apex run test --class-names {{TestClassName}} --target-org {{org}} --wait 10
```

## Run Specific Test Method
```bash
sf apex run test --tests {{TestClassName.methodName}} --target-org {{org}} --wait 10
```

## Run Tests with Code Coverage
```bash
sf apex run test --target-org {{org}} --code-coverage --wait 10
```

## Check Test Run Status
```bash
sf apex get test --test-run-id {{testRunId}} --target-org {{org}}
```

## View Apex Class List
```bash
sf data query --query "SELECT Id, Name, Status, IsValid, LengthWithoutComments FROM ApexClass ORDER BY Name" --target-org {{org}} --use-tooling-api
```

## Retrieve Apex Class
```bash
sf project retrieve start --metadata ApexClass:{{className}} --target-org {{org}}
```

## Deploy Apex Class
```bash
sf project deploy start --metadata ApexClass:{{className}} --target-org {{org}}
```
