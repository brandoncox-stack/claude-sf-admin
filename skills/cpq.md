# Salesforce CPQ (Configure, Price, Quote)

## Quotes

### List Quotes
```bash
sf data query --query "SELECT Id, Name, SBQQ__Opportunity2__r.Name, SBQQ__Status__c, SBQQ__Primary__c, SBQQ__NetAmount__c, SBQQ__ExpirationDate__c FROM SBQQ__Quote__c ORDER BY CreatedDate DESC LIMIT 20" --target-org {{org}}
```

### Get Quote Details
```bash
sf data query --query "SELECT Id, Name, SBQQ__Opportunity2__r.Name, SBQQ__Account__r.Name, SBQQ__Status__c, SBQQ__Primary__c, SBQQ__NetAmount__c, SBQQ__ListAmount__c, SBQQ__CustomerDiscount__c, SBQQ__StartDate__c, SBQQ__EndDate__c, SBQQ__ExpirationDate__c, SBQQ__SubscriptionTerm__c FROM SBQQ__Quote__c WHERE Id = '{{quoteId}}'" --target-org {{org}}
```

### List Quote Lines for a Quote
```bash
sf data query --query "SELECT Id, SBQQ__Product__r.Name, SBQQ__Quantity__c, SBQQ__ListPrice__c, SBQQ__NetPrice__c, SBQQ__Discount__c, SBQQ__Optional__c FROM SBQQ__QuoteLine__c WHERE SBQQ__Quote__c = '{{quoteId}}' ORDER BY SBQQ__Number__c" --target-org {{org}}
```

## Products & Price Books

### List Products
```bash
sf data query --query "SELECT Id, Name, ProductCode, IsActive, Family, Description FROM Product2 ORDER BY Name" --target-org {{org}}
```

### List Price Book Entries
```bash
sf data query --query "SELECT Id, Product2.Name, Pricebook2.Name, UnitPrice, IsActive FROM PricebookEntry WHERE Pricebook2.Name = '{{priceBookName}}' ORDER BY Product2.Name" --target-org {{org}}
```

### List CPQ Subscription Products
```bash
sf data query --query "SELECT Id, Name, SBQQ__SubscriptionPricing__c, SBQQ__SubscriptionTerm__c, SBQQ__SubscriptionType__c FROM Product2 WHERE SBQQ__SubscriptionPricing__c != null ORDER BY Name" --target-org {{org}}
```

## Bundles

### List Product Bundles
```bash
sf data query --query "SELECT Id, SBQQ__Bundle__r.Name, SBQQ__OptionalSKU__r.Name, SBQQ__Number__c, SBQQ__Quantity__c, SBQQ__Required__c, SBQQ__Type__c, SBQQ__Selected__c FROM SBQQ__ProductOption__c ORDER BY SBQQ__Bundle__r.Name, SBQQ__Number__c" --target-org {{org}}
```

### List Features for a Bundle
```bash
sf data query --query "SELECT Id, Name, SBQQ__Number__c, SBQQ__MinOptionCount__c, SBQQ__MaxOptionCount__c, SBQQ__ConfiguredSKU__r.Name FROM SBQQ__ProductFeature__c WHERE SBQQ__ConfiguredSKU__c = '{{bundleProductId}}' ORDER BY SBQQ__Number__c" --target-org {{org}}
```

### List Options for a Bundle
```bash
sf data query --query "SELECT Id, SBQQ__OptionalSKU__r.Name, SBQQ__Feature__r.Name, SBQQ__Number__c, SBQQ__Required__c, SBQQ__Selected__c, SBQQ__Quantity__c, SBQQ__Type__c FROM SBQQ__ProductOption__c WHERE SBQQ__ConfiguredSKU__c = '{{bundleProductId}}' ORDER BY SBQQ__Number__c" --target-org {{org}}
```

## Product Rules

### List Product Rules
```bash
sf data query --query "SELECT Id, Name, SBQQ__Type__c, SBQQ__Scope__c, SBQQ__EvaluationEvent__c, SBQQ__Active__c, SBQQ__ConditionsMet__c FROM SBQQ__ProductRule__c ORDER BY Name" --target-org {{org}}
```

### List Conditions for a Product Rule
```bash
sf data query --query "SELECT Id, SBQQ__TestedField__c, SBQQ__Operator__c, SBQQ__FilterValue__c, SBQQ__FilterType__c FROM SBQQ__ErrorCondition__c WHERE SBQQ__Rule__c = '{{ruleId}}'" --target-org {{org}}
```

### List Price Rules
```bash
sf data query --query "SELECT Id, Name, SBQQ__TargetObject__c, SBQQ__EvaluationEvent__c, SBQQ__Active__c, SBQQ__ConditionsMet__c FROM SBQQ__PriceRule__c ORDER BY Name" --target-org {{org}}
```

### List Price Conditions/Actions for a Price Rule
```bash
sf data query --query "SELECT Id, SBQQ__Field__c, SBQQ__Operator__c, SBQQ__Value__c FROM SBQQ__PriceCondition__c WHERE SBQQ__Rule__c = '{{priceRuleId}}'" --target-org {{org}}
sf data query --query "SELECT Id, SBQQ__TargetObject__c, SBQQ__Field__c, SBQQ__Value__c, SBQQ__Formula__c FROM SBQQ__PriceAction__c WHERE SBQQ__Rule__c = '{{priceRuleId}}'" --target-org {{org}}
```

## Discount Schedules

### List Discount Schedules
```bash
sf data query --query "SELECT Id, Name, SBQQ__Type__c, SBQQ__DiscountUnit__c FROM SBQQ__DiscountSchedule__c ORDER BY Name" --target-org {{org}}
```

### View Discount Tiers
```bash
sf data query --query "SELECT Id, Name, SBQQ__LowerBound__c, SBQQ__UpperBound__c, SBQQ__Discount__c FROM SBQQ__DiscountTier__c WHERE SBQQ__Schedule__c = '{{scheduleId}}' ORDER BY SBQQ__LowerBound__c" --target-org {{org}}
```

## CPQ Settings
```bash
sf org open --target-org {{org}} --path /lightning/n/SBQQ__EditSettings
```

## Retrieve CPQ Metadata
```bash
sf project retrieve start --metadata CustomObject:SBQQ__Quote__c --target-org {{org}}
sf project retrieve start --metadata CustomObject:SBQQ__QuoteLine__c --target-org {{org}}
sf project retrieve start --metadata CustomObject:SBQQ__ProductOption__c --target-org {{org}}
sf project retrieve start --metadata CustomObject:SBQQ__ProductRule__c --target-org {{org}}
```

## Notes
- CPQ objects use the SBQQ__ namespace prefix
- Bundle = parent product with Product Options (child products)
- Features organize options within a bundle
- Product Rules: Validation, Selection, Alert, Filter
- Price Rules: automate pricing calculations
- Discount Schedules: volume/term-based discounting
