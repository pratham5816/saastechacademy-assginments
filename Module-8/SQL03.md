


# SQL-ASSIGNMENT-03

## Q1 Completed Sales Orders (Physical Items)

```sql
SELECT OH.ORDER_ID,
OI.ORDER_ITEM_SEQ_ID,
OI.PRODUCT_ID,
P.product_type_id,
OH.SALES_CHANNEL_ENUM_ID,
OH.ORDER_DATE,
OH.ENTRY_DATE,
OH.STATUS_ID,
OH.ORDER_TYPE_ID,
OH.PRODUCT_STORE_ID
FROM order_header OH
JOIN ORDER_ITEM OI ON OH.order_id = OI.order_id
JOIN product P ON oi.PRODUCT_ID = P.product_id 
WHERE OH.ORDER_TYPE_ID = 'SALES_ORDER';
```

## Q5 Detailed Return Information

```sql
SELECT RH.RETURN_ID,
       RH.ENTRY_DATE,
       RH.RETURN_HEADER_TYPE_ID,
       RI.RETURN_PRICE,
       OI.COMMENTS,
       OI.ORDER_ID,
       O.ORDER_DATE,
       RH.RETURN_DATE,
       O.PRODUCT_STORE_ID
FROM return_header RH
JOIN RETURN_ITEM RI ON RH.RETURN_ID = RI.RETURN_ID
JOIN ORDER_HEADER O ON O.ORDER_ID = RI.ORDER_ID
JOIN ORDER_ITEM OI ON RI.order_id = OI.order_Id and RI.ORDER_ITEM_SEQ_ID = OI.ORDER_ITEM_SEQ_ID;

```

## Q8 List of Warehouse Pickers

```sql
SELECT PR.party_id,
P.first_name,
P.last_name,
PR.role_type_id,
FP.facility_id,
CASE
WHEN (FP.from_date <= CURRENT_TIMESTAMP()
AND (FP.thru_date IS NULL
OR FP.thru_date >= CURRENT_TIMESTAMP())) THEN 'ACTIVE'
ELSE 'INACTIVE'
END
FROM party_role PR
JOIN PERSON P ON PR.party_id = P.party_id
JOIN FACILITY_PARTY FP ON FP.party_id = P.party_id
WHERE PR.role_type_id = 'WAREHOUSE_PICKER';
```

## Q9 Total Facilities That Sell the Product

```sql
SELECT P.PRODUCT_ID,
P.internal_name,
count(PF.FACILITY_ID)
FROM product P
JOIN product_facility PF ON P.product_id = PF.product_id
GROUP BY P.PRODUCT_ID,
P.Internal_name;
```

## Q10 Total Items in Various Virtual Facilities

```sql
SELECT II.PRODUCT_ID,
II.FACILITY_ID,
F.FACILITY_TYPE_ID,
II.AVAILABLE_TO_PROMISE_TOTAL,
II.QUANTITY_ON_HAND_TOTAL
FROM inventory_item II
JOIN Facility F ON II.FACILITY_ID = F.FACILITY_ID;
```
