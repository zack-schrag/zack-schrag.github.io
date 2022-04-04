---
layout: default
title: Using Configuration to Cleanly Swap Dependencies in .NET
categories: csharp
tags: configuration csharp c#
excerpt_separator: <!--more-->
---

Here are some patterns you can use when you need to support swapping underlying technologies, or being proactive if and when that need arises.

<!--more-->

Let's say you're writing an application to get transactions from your personal bank account. Each time you run your application, it will interact with only one bank.
```csharp
public interface IBankAccountProvider {
    (List<Transaction>, BankAccountErrorType?) GetTransactions();
}

public class WellsFargoProvider : IBankAccountProvider {
    public WellsFargoProvider(
        ISomeDependencySpecificToWellsFargo dependencyA,
        IAnotherDependencySpecificToWellsFargo dependencyB) {
        // ...
    }

    (List<Transaction>, BankAccountErrorType?) GetTransactions() {
        // ...
    }
}

public class UsBankProvider : IBankAccountProvider {
    public WellsFargoProvider(
        ISomeDependencySpecificToUsBank dependencyA,
        IAnotherDependencySpecificToUsBank dependencyB) {
        // ...
    }

    (List<Transaction>, BankAccountErrorType?) GetTransactions() {
        // ...
    }
}
```
## Application runtime needs one type of provider
### Using extension methods
### Using extension methods with configuration

## Application runtime needs to support multiple providers
### Using a factory class

