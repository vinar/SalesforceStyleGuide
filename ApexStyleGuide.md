# Apex Style Guide #

<!-- MarkdownTOC depth=0 autolink=true autoanchor=true bracket=round -->

- [Intro](#intro)
  - [Sources](#sources)
- [Basics](#basics)
  - [Special characters](#special-characters)
    - [Whitespace](#whitespace)
    - [Special escape sequences](#special-escape-sequences)
    - [Other Non-ASCII Characters](#other-non-ascii-characters)
- [Structure](#structure)
  - [Indentation](#indentation)
  - [Comments](#comments)
  - [New-lines and spaces](#new-lines-and-spaces)
  - [Prefer Explicit Declarations](#prefer-explicit-declarations)
  - [`@isTest`](#istest)
  - [Capitalization](#capitalization)
- [SOQL](#soql)
- [Apex-Specific SObject Constructor Syntax](#apex-specific-sobject-constructor-syntax)
- [Test.startTest() and Test.stopTest()](#teststarttest-and-teststoptest)
- [Naming Conventions](#naming-conventions)
  - [Class](#class)
  - [Trigger](#trigger)
  - [Methods](#methods)
  - [Test classes](#test-classes)
- [Example Code](#example-code)
  - [Apex Class](#apex-class-example)
  - [Test Class](#test-class-example)
  - [Trigger](#trigger-example)

<!-- /MarkdownTOC -->

<a name="intro"></a>
## Intro

<a name="sources"></a>
### Sources
This started as a clone of [PolarisProject Style Guide](https://github.com/PolarisProject/salesforceStyleGuide) 

<a name="basics"></a>
## Basics
<a name="special-characters"></a>
### Special characters
<a name="whitespace"></a>
#### Whitespace
The only permissible whitespace characters in source code are newline and space (0x20).  Inside of a string literal, only a space is allowed.  Line must not end with spaces (`/ +$/` must not match anything in the file).  Classes should all end with a newline.

<a name="special-escape-sequences"></a>
#### Special escape sequences
For any character that has a special escape sequence (`\b`, `\t`, `\n`, `\f`, `\r`, `\"`, `\'` and `\\`), that sequence is used rather than the corresponding octal (e.g. `\012`) or Unicode (e.g. `\u000a`) escape.

<a name="other-non-ascii-characters"></a>
#### Other Non-ASCII Characters
For the remaining non-ASCII characters, either the actual Unicode character (e.g. `∞`) or the equivalent Unicode escape (e.g. `\u221e`) is used, depending only on which makes the code easier to read and understand.

  > Tip: In the Unicode escape case, and occasionally even when actual Unicode characters are used, an explanatory comment can be very helpful.

<a name="structure"></a>
## Structure

The ordering of the members of a class can have a great effect on learnability, but there is no single correct recipe for how to do it. Different classes may order their members differently.

What is important is that each class order its members in some logical order, which its maintainer could explain if asked. For example, new methods are not just habitually added to the end of the class, as that would yield "chronological by date added" ordering, which is not a logical ordering.

<a name="indentation"></a>
### Indentation
All blocks of code should be indented with 4 spaces.  Spaces, not tabs, to ensure that it looks the same on everyone's screen.

<a name="comments"></a>
### Comments
Prefer placing comments on a line by themselves. Single line comments use double forward slash's `//` followed by a space.
  >  `// Here is a single line comment`
  
  >  `/* Don't add single line comments this way */`

<a name="new-lines-and-spaces"></a>
### New-lines and spaces
Open braces should have a space before them and not a newline.  The matching close brace should line up with the start of the opening brace's line.

The parenthetical clause in `if`, `while`, `do`, `catch`, etc., statements should be preceded and followed by a single space.
>e.g., `if (a == b) {`

In method definitions, there should be no space before the open parenthesis, and one space after.
>e.g., `public void myMethod() {`

In method calls and definitions, there should not be whitespace between the name of the method and the open parenthesis.
>e.g., `obj.myMethod();`

A single space should separate binary operators (e.g., `+`, `||`, `=`, `>=`) from the surrounding elements. 
>e.g., `if (a || b)`

Unary operators (`!`, `-`) should be attached to their parameters. e.g., `if (!something)`

A colon inside a `for each` loop should have one space on either side. 
>e.g., `for (Contact cnt : contacts) {`

There should be no whitespace before commas, and one space after. 
>e.g., `System.debug(LoggingLevel.INFO, 'some text');`

If using C#-style properties, code should follow the following rules:

 * Always declare the getter, then the setter.
 * If there is no logic, it should read `{ get; set; }`.
 * If there is logic, there should be a new-line before each open-brace, and before and after each closed-brace.
 * If one clause has logic and one does not, place the clause without logic on its own line.

<a name="prefer-explicit-declarations"></a>
### Prefer Explicit Declarations
Always specify:

* `global`/`public`/`private` modifiers - prefer `private`, and if possible, `static`
* `with sharing`/`without sharing`

<a name="istest"></a>
### `@isTest`
In a test method, use the `@isTest` attribute instead of the `testmethod` modifier.

<a name="capitalization"></a>
### Capitalization

Follow the Java standard of capitalization with the listed exceptions.  That means that statements (`for`, `if`, etc.) should be lowercase, constants should be `UPPER_CASE_WITH_UNDERSCORES`, classes and class-level variables should be declared as `UpperCamelCase`, and methods, parameters and local variables should all be declard as `lowerCamelCase`.

Native Apex methods and classes should generally be referenced as written in official Salesforce documentation.  This means that schemas and classes are `UpperCamelCase` and methods are `lowerCamelCase`.  The only deviation from this rule is `SObject` which should be written as such (in the documentation, it is usually written `sObject` which does not conform to this style guide and should not be used).

However, when referencing any metadata (SObject, SObjectField, FieldSet, Action, Class, Page, etc.), use the declared capitalization.  Even when referencing a method, field, etc., that is not capitalized according to these rules, still use the declared capitalization.


<a name="soql"></a>
## SOQL

In general, SOQL should be declared inline where it is used.  In some cases, like when referencing FieldSets, it's necessary to build SOQL queries dynamically.  The same rules will generally apply.

SOQL keywords (e.g., `SELECT`, `WHERE`, `TODAY`) should always be written in `ALL CAPS`.  Objects, fields and bind variables should be referenced as declared.  Each clause of the SOQL Query should be on its own line so that finding what changed in a diff is easier.  That is, each `SELECT`, `FROM`, `WHERE`, `AND`, `OR`, `GROUP BY`, `HAVING`, `ROLL UP`, `ORDER BY`, etc., with the exception of the first `SELECT` should start a new line.  That line should start in the same column as the most relevant `SELECT`.

Long lists of fields in a `SELECT` clause should be ordered in a logical manner and broken to fit within page width, with subsequent lines aligned with the first field.  Always select `Id`, and always select it first.

Example (in context):

```java
String typeToSelect = 'Email';
List<Contact> contacts = [
  SELECT Id, FirstName, LastName, Phone, MobilePhone, Email, Salutation, Title, Department,
    MailingCity, MailingState, MailingCountry, MailingPostalCode,
    (SELECT Id, ActivityDate, Origin, Type, WhatId, What.Name, RecordTypeId
     FROM ActivityHistories
     WHERE Type = :typeToSelect)
  FROM Contact
  WHERE CreatedDate >= TODAY];
```

<a name="apex-specific-sobject-constructor-syntax"></a>
## Apex-Specific SObject Constructor Syntax
When creating an SObject, generally prefer the Apex-specific syntax wherein all fields can be initialized from the constructor.  When using this syntax, choose a different line for each property so that diff-ing and versioning is easier.

Example:

```java
Contact c = new Contact(
  RecordTypeId = CONTACT_RECORDTYPE_ID,
  FirstName = firstName,
  LastName = surname,
  MailingCountry = DEFAULT_COUNTRY
);
```

<a name="teststarttest-and-teststoptest"></a>
## Test.startTest() and Test.stopTest()
When writing test cases, always use `Test.startTest();` and `Test.stopTest();`.  Do not indent the code between those method calls, but do use one line of vertical whitespace above and below those method calls to seprate those lines from surrounding code.

<a name="naming-conventions"></a>
## Naming Conventions

<a name="class"></a>
### Class
Name a class or trigger after what it does (e.g., `AccountTriggerHandler`). Controllers and Controller Extensions should end with the word `Controller`.  Use `UpperCamelCase`.

<a name="trigger"></a>
### Trigger
Name a trigger after the SObject it operates against.  Triggers should be named with a combination of the SObject type followed by the word `Trigger` (e.g., `AccountTrigger`, `OpportunityTrigger`, `SomeCustomObjectTrigger`).  Triggers should not contain any logic, that should be left to the Handler class. Use `UpperCamelCase`.

<a name="methods"></a>
### Methods
Methods should all be verbs.  Getters and setters should have no side effects (with the exception of setting up cached values and/or logging), and should begin with `get` or `set`.

<a name="test-classes"></a>
### Test classes
Test classes should be named `MyClassTest`.  If the test is not a unit-level test but instead a broader test case, it it should be named `StuffBeingTestedTest`.

<a name="example-code"></a>
## Example Code

<a name="apex-class-example"></a>
### Apex Class Example

```java
public class MyClass {

    private final static Integer SOME_CONSTANT = 3;
    private final static Integer SOME_OTHER_CONSTANT = 7;
    private final static Integer MAX_INTS = 5;

    private Contact internallyUsedContact { get; set; }

    public Integer calculatedInteger {
        get {
            return 5;
        }
        set {
            internallyUsedContact = [
                SELECT Id
                FROM Contact
                WHERE Number_of_Peanuts__c > :value
                LIMIT 1];
        }
    }

    private Id contactId {
        get;
        set {
            System.debug('Why do this?');
            this.contactId = value;
        }
    }

    public void foo(Integer bar) {
        // Add some comment about this
        if (bar == SOME_CONSTANT) {
            System.debug(debugCode(bar) + ' - hi there!');
            return;
        } 
        // And another comment here
        else if (bar > SOME_OTHER_CONSTANT) {
            List<Integer> wasteOfSpace = new List<Integer>();
            do {
                wasteOfSpace.add(calculatedInteger);
            } while (wasteOfSpace.size() < MAX_INTS);
        } 
        else {
            try {
                upsert v;
            } catch (Exception ex) {
                handleException(ex);
            }
        }

        for (Integer i : wasteOfSpace) {
            System.debug('Here\'s an integer! ' + i);
        }
    }  
}
```

<a name="test-class-example"></a>
## Test Class Example

```java
@isTest
private class AccountServiceTest {

    @isTest static void testSetAccountType() {
        Account acct = new Account(Name='Test1');

        Test.startTest();
	
        AccountService.setAccountType(new List<Account> { acct });
	
        Test.stopTest();

        System.assertEquals(AccountService.ACCOUNT_TYPE_CUSTOMER, acct.Type);
    }
}
```

<a name="trigger-example"></a>
## Trigger & Related Classes Example

```java
trigger AccountTrigger on Account (before insert, before update, before delete,
								after insert, after update, after delete, after undelete) {

	AccountTriggerHandler handler = new AccountTriggerHandler(Trigger.isExecuting, Trigger.size);

	if (Trigger.isInsert && Trigger.isBefore) {
		handler.onBeforeInsert(Trigger.new, Trigger.newMap);
	}
	else if (Trigger.isInsert && Trigger.isAfter) {
		handler.onAfterInsert(Trigger.new, Trigger.newMap);
	}
	else if (Trigger.isUpdate && Trigger.isBefore) {
		handler.onBeforeUpdate(Trigger.new, Trigger.newMap, Trigger.old, Trigger.oldMap);
	}
	else if (Trigger.isUpdate && Trigger.isAfter) {
		handler.onAfterUpdate(Trigger.new, Trigger.newMap, Trigger.old, Trigger.oldMap);
	}
	else if (Trigger.isDelete && Trigger.isBefore) {
		handler.onBeforeDelete(Trigger.old, Trigger.oldMap);
	}
	else if (Trigger.isDelete && Trigger.isAfter) {
		handler.onAfterDelete(Trigger.old, Trigger.oldMap);
	}
	else if (Trigger.isUndelete) {
		handler.onUndelete(Trigger.new);
	}
} 
```

```java
public with sharing class AccountTriggerHandler {

    private Boolean isExecutingFlag = false;
    private Integer batchSize = 0;
    public static Boolean firstRun = true;

    public AccountTriggerHandler(Boolean isExecuting, Integer size) {
        isExecutingFlag = isExecuting;
        batchSize = size;
    }

    public void onBeforeInsert(List<Account> currentAccounts, Map<Id, Account> currentAccountsMap) {
        if (firstRun) {
            firstRun = false;
            
            // Call additional methods here. 
            // example
            AccountService.setAccountType(currentAccounts);
        }
    }

    public void onAfterInsert(List<Account> currentAccounts, Map<Id, Account> currentAccountsMap) {
        // Call additional methods here. 		
    }

    public void onBeforeUpdate(List<Account> currentAccounts, Map<Id, Account> currentAccountsMap, List<Account> oldAccounts, Map<Id, Account> oldAccountsMap) {
        if (firstRun) {
            firstRun = false;
	
            // Call additional methods here
        }
    }

    public void onAfterUpdate(List<Account> currentAccounts, Map<Id, Account> currentAccountsMap, List<Account> oldAccounts, Map<Id, Account> oldAccountsMap) {
        // Call additional methods here		
    }

    public void onBeforeDelete(List<Account> oldAccounts, Map<Id, Account> oldAccountsMap) {
        if (firstRun) {
            firstRun = false;

            // Call additional methods here
        }
    }

    public void onAfterDelete(List<Account> oldAccounts, Map<Id, Account> oldAccountsMap) {
        // Call additional methods here
    }

    public void onUndelete(List<Account> currentAccounts) {
        if (firstRun) {
            firstRun = false;

            // Call additional methods here
        }
    }

    public Boolean IsTriggerContext {
        get { return isExecutingFlag; }
    }

    public Boolean IsVisualforcePageContext {
        get { return !IsTriggerContext; }
    }

    public Boolean IsWebServiceContext {
        get { return !IsTriggerContext; }
    }

    public Boolean IsExecuteAnonymousContext {
        get { return !IsTriggerContext; }
    }
} 

```

```java
public without sharing class AccountService {

    public final static String ACCOUNT_TYPE_CUSTOMER = 'Customer';

    // Set the Account Type upon creation
    public static void setAccountType(List<Account> currentAccounts) {
        for (Account a : currentAccounts) {
            a.Type = ACCOUNT_TYPE_CUSTOMER;
        }
    }
} 
```

