# Salesforce Billing (Orders, Invoicing, Subscriptions, Renewals)

## Orders

### List Orders
```bash
sf data query --query "SELECT Id, OrderNumber, Account.Name, Status, TotalAmount, EffectiveDate, Type FROM Order ORDER BY CreatedDate DESC LIMIT 20" --target-org {{org}}
```

### Get Order Details
```bash
sf data query --query "SELECT Id, OrderNumber, Account.Name, Status, TotalAmount, EffectiveDate, EndDate, Type, SBQQ__Quote__r.Name FROM Order WHERE Id = '{{orderId}}'" --target-org {{org}}
```

### List Order Products
```bash
sf data query --query "SELECT Id, Product2.Name, Quantity, UnitPrice, TotalPrice, blng__BillingRule__r.Name, blng__RevenueRecognitionRule__r.Name, blng__TaxRule__r.Name FROM OrderItem WHERE OrderId = '{{orderId}}'" --target-org {{org}}
```

### Activate an Order
```bash
sf data update record --sobject Order --record-id {{orderId}} --values "Status='Activated'" --target-org {{org}}
```

## Invoices

### List Invoices
```bash
sf data query --query "SELECT Id, Name, blng__Account__r.Name, blng__InvoiceStatus__c, blng__TotalAmount__c, blng__DueDate__c, blng__InvoiceDate__c FROM blng__Invoice__c ORDER BY CreatedDate DESC LIMIT 20" --target-org {{org}}
```

### Get Invoice Details
```bash
sf data query --query "SELECT Id, Name, blng__Account__r.Name, blng__InvoiceStatus__c, blng__TotalAmount__c, blng__Balance__c, blng__DueDate__c, blng__InvoiceDate__c, blng__PaymentTerm__c FROM blng__Invoice__c WHERE Id = '{{invoiceId}}'" --target-org {{org}}
```

### List Invoice Lines
```bash
sf data query --query "SELECT Id, Name, blng__Product__r.Name, blng__Quantity__c, blng__TotalAmount__c, blng__StartDate__c, blng__EndDate__c FROM blng__InvoiceLine__c WHERE blng__Invoice__c = '{{invoiceId}}'" --target-org {{org}}
```

### Post an Invoice
```bash
sf data update record --sobject blng__Invoice__c --record-id {{invoiceId}} --values "blng__InvoiceStatus__c='Posted'" --target-org {{org}}
```

## Billing Rules, Revenue Recognition & Tax Rules

### List Billing Rules
```bash
sf data query --query "SELECT Id, Name, blng__InitialBillingTrigger__c, blng__NextBillingTrigger__c, blng__GenerateInvoices__c FROM blng__BillingRule__c ORDER BY Name" --target-org {{org}}
```

### List Revenue Recognition Rules
```bash
sf data query --query "SELECT Id, Name, blng__RevenueRecognitionTreatment__c, blng__RevenueScheduleTermStartDate__c, blng__RevenueScheduleTermEndDate__c FROM blng__RevenueRecognitionRule__c ORDER BY Name" --target-org {{org}}
```

### List Tax Rules
```bash
sf data query --query "SELECT Id, Name, blng__TaxableHome__c FROM blng__TaxRule__c ORDER BY Name" --target-org {{org}}
```

## Payments

### List Payments
```bash
sf data query --query "SELECT Id, Name, blng__Account__r.Name, blng__Amount__c, blng__Status__c, blng__PaymentDate__c, blng__PaymentType__c FROM blng__Payment__c ORDER BY CreatedDate DESC LIMIT 20" --target-org {{org}}
```

### Allocate Payment to Invoice
```bash
sf data create record --sobject blng__PaymentAllocationInvoiceLine__c --values "blng__Payment__c='{{paymentId}}' blng__InvoiceLine__c='{{invoiceLineId}}' blng__Amount__c={{amount}}" --target-org {{org}}
```

## Credit Notes

### List Credit Notes
```bash
sf data query --query "SELECT Id, Name, blng__Account__r.Name, blng__TotalAmount__c, blng__Status__c, blng__CreditNoteDate__c FROM blng__CreditNote__c ORDER BY CreatedDate DESC LIMIT 20" --target-org {{org}}
```

## Subscriptions & Renewals

### List Active Contracts
```bash
sf data query --query "SELECT Id, ContractNumber, Account.Name, Status, StartDate, EndDate, SBQQ__RenewalOpportunityStage__c FROM Contract WHERE Status = 'Activated' ORDER BY EndDate" --target-org {{org}}
```

### List Subscriptions
```bash
sf data query --query "SELECT Id, Name, SBQQ__Account__r.Name, SBQQ__Product__r.Name, SBQQ__Quantity__c, SBQQ__NetPrice__c, SBQQ__StartDate__c, SBQQ__EndDate__c, SBQQ__Contract__r.ContractNumber FROM SBQQ__Subscription__c ORDER BY SBQQ__EndDate__c LIMIT 20" --target-org {{org}}
```

### Contracts Up for Renewal (next 30/60/90 days)
```bash
sf data query --query "SELECT Id, ContractNumber, Account.Name, EndDate, SBQQ__RenewalQuoted__c FROM Contract WHERE Status = 'Activated' AND EndDate = NEXT_N_DAYS:{{days}} ORDER BY EndDate" --target-org {{org}}
```

### List Assets
```bash
sf data query --query "SELECT Id, Name, Product2.Name, Account.Name, Quantity, Status, InstallDate FROM Asset WHERE Account.Name = '{{accountName}}' ORDER BY Name" --target-org {{org}}
```

## Billing Config Setup Pages
```bash
# Billing Rules
sf org open --target-org {{org}} --path /lightning/o/blng__BillingRule__c/list
# Revenue Recognition Rules
sf org open --target-org {{org}} --path /lightning/o/blng__RevenueRecognitionRule__c/list
# Tax Rules
sf org open --target-org {{org}} --path /lightning/o/blng__TaxRule__c/list
```

## Notes
- Billing objects use the blng__ namespace prefix
- CPQ objects use the SBQQ__ namespace prefix
- Order flow: Quote → Order → Invoice → Payment
- Billing Rules control WHEN invoices generate (Order Activation, Order Product Activation)
- Revenue Recognition Rules control HOW revenue is recognized
- Tax Rules define tax handling per product
- Contracts auto-generate renewal Opportunities when nearing end date
