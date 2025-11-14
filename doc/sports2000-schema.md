# Sports2000 Database Schema Summary

## Overview

The Sports2000 database is a sample database that demonstrates a sporting goods retail business.

## Key Tables

### Customer
- **CustNum** (INTEGER): Primary key, customer number
- **Name** (CHARACTER): Customer name
- **Address**, **Address2** (CHARACTER): Street addresses
- **City** (CHARACTER): City name
- **State** (CHARACTER): State/province
- **Country** (CHARACTER): Country (default: USA)
- **PostalCode** (CHARACTER): Postal/ZIP code
- **Contact** (CHARACTER): Contact person
- **Phone** (CHARACTER): Phone number
- **SalesRep** (CHARACTER): Assigned sales representative
- **CreditLimit** (DECIMAL): Maximum credit allowed
- **Balance** (DECIMAL): Current balance
- **Terms** (CHARACTER): Payment terms (default: Net30)
- **Discount** (INTEGER): Discount percentage
- **Comments** (CHARACTER): Additional notes
- **Fax** (CHARACTER): Fax number
- **EmailAddress** (CHARACTER): Email address

### Item
- **ItemNum** (INTEGER): Primary key, item number
- **ItemName** (CHARACTER): Item name
- **Price** (DECIMAL): Item price
- **OnHand** (INTEGER): Quantity on hand
- **Allocated** (INTEGER): Quantity allocated
- **ReOrder** (INTEGER): Reorder level
- **OnOrder** (INTEGER): Quantity on order
- **CatPage** (INTEGER): Catalog page
- **CatDescription** (CHARACTER): Catalog description
- **Category1**, **Category2** (CHARACTER): Item categories
- **Special** (CHARACTER): Special designation
- **Weight** (DECIMAL): Item weight
- **Minqty** (INTEGER): Minimum order quantity

### Order
- **OrderNum** (INTEGER): Primary key, order number
- **CustNum** (INTEGER): Foreign key to Customer
- **OrderDate** (DATE): Order date
- **ShipDate** (DATE): Ship date
- **PromiseDate** (DATE): Promised delivery date
- **Carrier** (CHARACTER): Shipping carrier
- **Instructions** (CHARACTER): Shipping instructions
- **PO** (CHARACTER): Purchase order number
- **Terms** (CHARACTER): Payment terms
- **SalesRep** (CHARACTER): Sales representative
- **BillToID** (INTEGER): Billing address ID
- **ShipToID** (INTEGER): Shipping address ID
- **OrderStatus** (CHARACTER): Order status
- **WarehouseNum** (INTEGER): Warehouse number
- **Creditcard** (CHARACTER): Credit card type

### OrderLine
- **OrderNum** (INTEGER): Foreign key to Order
- **LineNum** (INTEGER): Line number
- **ItemNum** (INTEGER): Foreign key to Item
- **Price** (DECIMAL): Line item price
- **Qty** (INTEGER): Quantity ordered
- **Discount** (INTEGER): Discount percentage
- **ExtendedPrice** (DECIMAL): Extended price
- **OrderLineStatus** (CHARACTER): Line status

## Sequences

- **NextCustNum**: Customer number sequence (starts at 1000, increment 5)
- **NextOrdNum**: Order number sequence (starts at 1000, increment 5)
- **NextItemNum**: Item number sequence (starts at 100, increment 10)
- **NextInvNum**: Invoice number sequence (starts at 1000, increment 1)

## Additional Tables

- **BillTo**: Billing addresses for customers
- **ShipTo**: Shipping addresses for customers
- **Salesrep**: Sales representative information
- **Warehouse**: Warehouse locations
- **Bin**: Inventory bins within warehouses
- **Benefits**: Employee benefits
- **Employee**: Employee information

## Usage

For complete schema definition, see `dump/sports2000.df`.
