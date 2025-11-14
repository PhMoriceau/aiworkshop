# ABL Business Entity Architecture Pattern

## Overview

The Business Entity pattern provides a standardized, maintainable approach to data access in OpenEdge ABL applications. It separates UI logic from database operations through a layered architecture that promotes reusability, testability, and consistency.

## Architecture Layers

### 1. UI Layer (Windows/Forms)
- **Responsibility**: User interaction and presentation
- **Access**: Never directly accesses database tables
- **Communication**: Calls Business Entity methods with datasets

### 2. Business Entity Layer
- **Responsibility**: Data access, business rules, validation
- **Inheritance**: Extends `OpenEdge.BusinessLogic.BusinessEntity`
- **Management**: Instantiated through EntityFactory (singleton pattern)

### 3. Database Layer
- **Responsibility**: Persistent storage
- **Access**: Only through data-sources attached to business entities

## Key Components

### EntityFactory (Singleton Pattern)

```abl
CLASS business.EntityFactory:
    VAR PRIVATE STATIC EntityFactory objInstance.
    VAR PRIVATE CustomerEntity objCustomerEntityInstance.
    
    CONSTRUCTOR PRIVATE EntityFactory():
    END CONSTRUCTOR.
    
    METHOD PUBLIC STATIC EntityFactory GetInstance():
        IF objInstance = ? THEN
            objInstance = NEW EntityFactory().
        RETURN objInstance.
    END METHOD.
    
    METHOD PUBLIC CustomerEntity GetCustomerEntity():
        IF objCustomerEntityInstance = ? THEN
            objCustomerEntityInstance = NEW CustomerEntity().
        RETURN objCustomerEntityInstance.
    END METHOD.
END CLASS.
```

### Dataset Definition (.i Include Files)

```abl
DEFINE TEMP-TABLE ttCustomer BEFORE-TABLE bttCustomer
    FIELD CustNum AS INTEGER INITIAL "0" LABEL "Cust Num"
    FIELD Name AS CHARACTER LABEL "Name"
    /* ... additional fields ... */
    INDEX CustNum IS PRIMARY UNIQUE CustNum ASCENDING.

DEFINE DATASET dsCustomer FOR ttCustomer.
```

### Business Entity Class

```abl
CLASS business.CustomerEntity INHERITS BusinessEntity USE-WIDGET-POOL:
    {business/CustomerDataset.i}
    DEFINE DATA-SOURCE srcCustomer FOR Customer.
    
    CONSTRUCTOR PUBLIC CustomerEntity():
        SUPER(DATASET dsCustomer:HANDLE).
        VAR HANDLE[1] hDataSourceArray = DATA-SOURCE srcCustomer:HANDLE.
        VAR CHARACTER[1] cSkipListArray = [""].
        THIS-OBJECT:ProDataSource = hDataSourceArray.
        THIS-OBJECT:SkipList = cSkipListArray.
    END CONSTRUCTOR.
END CLASS.
```

## Standard CRUD Operations

### Read Operations

```abl
METHOD PUBLIC LOGICAL GetCustomerByNumber(INPUT ipiCustNum AS INTEGER,
                                          OUTPUT DATASET dsCustomer):
    VAR CHARACTER cFilter = "WHERE Customer.CustNum = " + STRING(ipiCustNum).
    THIS-OBJECT:ReadData(cFilter).
    RETURN CAN-FIND(FIRST ttCustomer).
END METHOD.
```

**Important**: Use `OUTPUT DATASET` (NOT `BY-REFERENCE`) for read operations.

### Update Operations

```abl
METHOD PUBLIC VOID UpdateCustomer(INPUT-OUTPUT DATASET dsCustomer):
    THIS-OBJECT:UpdateData(DATASET dsCustomer BY-REFERENCE).
END METHOD.
```

**Usage**:
```abl
/* Enable change tracking before modifications */
TEMP-TABLE ttCustomer:TRACKING-CHANGES = TRUE.
FIND FIRST ttCustomer.
ttCustomer.Name = "Updated Name".
objEntity:UpdateCustomer(INPUT-OUTPUT DATASET dsCustomer BY-REFERENCE).
```

## UI Integration Pattern

```abl
USING business.CustomerEntity FROM PROPATH.
USING business.EntityFactory FROM PROPATH.
{business/CustomerDataset.i}

ON CHOOSE OF GetCustomer:
    VAR INTEGER iCustomerNumber = INTEGER(CustomerNumber:screen-value).
    VAR EntityFactory objFactory = EntityFactory:GetInstance().
    VAR CustomerEntity objEntity = objFactory:GetCustomerEntity().
    VAR LOGICAL lFound.
    
    lFound = objEntity:GetCustomerByNumber(iCustomerNumber, OUTPUT DATASET dsCustomer).
    
    IF lFound THEN DO:
        FIND FIRST ttCustomer.
        CustomerName = ttCustomer.Name.
        DISPLAY CustomerName.
    END.
END.
```

## Common Pitfalls

### 1. Using BY-REFERENCE on OUTPUT DATASET for Read Operations
**Wrong**: `OUTPUT DATASET dsCustomer BY-REFERENCE`
**Correct**: `OUTPUT DATASET dsCustomer`

### 2. Forgetting Change Tracking for Updates
**Always enable**: `TEMP-TABLE ttCustomer:TRACKING-CHANGES = TRUE.`

### 3. Not Using Named Buffers
**Wrong**: `FOR EACH Customer...`
**Correct**: `DEFINE BUFFER bCustomer FOR Customer. FOR EACH bCustomer...`

## Benefits

- **Maintainability**: Centralized data access logic
- **Reusability**: Business entities shared across UI components
- **Testability**: Business logic isolated from UI
- **Consistency**: All data access follows same pattern
- **Scalability**: Easy to add new entities

## References

- Project Examples:
  - `src/business/CustomerEntity.cls`
  - `src/business/EntityFactory.cls`
  - `src/CustomerWin.w`
