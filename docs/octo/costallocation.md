# Cost Allocation

Octo allows the distribution of costs through cost allocation. Cost allocation covers three areas of distributing costs;

1. Allocation for enterprise support and other fee item in AWS
2. Allocation of specific Account usage to another accounts
3. Allocation for RI and SP savings

## Allocator
Allocator manages the cost allocators. Cost allocators are processed daily on the batch. 

Each cost allocator is composed of a json format as below.
``` sh
{
  "category": "",
  "expiration": "",
  "startMonth": "",
  "defaultAccount": "",
  "criteria": [],
  "allocator": []
}
```

`category` - Category specifies the area of cost allocation where the cost allocator is to be applied. There are three categories for now, "fee", "account" and "savings". 
If category is not specified, the cost allocator is automatically assigned with fee category.
``` sh
"category": "fee",
```

`expiration` - Expiration refers to the number of months wherein the cost allocator is active.  The count starts from the time the allocator is created. 
If this is not set, this means that the allocator won't expire and it is to be applied always.
``` sh
"expiration": "3",
```

`startMonth` - StartMonth refers to the month when the allocation is started. If a fee is to be distributed to a number of months, the month it will reflects first on the account is based on the startMonth value.
If this is not set, the current month will be set as the starting month.
``` sh
"startMonth": "2024-01",
```

`defaultAccount` - In case there are still some remaining values after cost allocations, it will be applied to the account specified on the defaultAccount.
If this is not set, the original or the source account will serve as the default account.
``` sh
"defaultAccount": "123456789012",
```

`criteria` - Using the values under criteria, it will determine which items are to be allocated. If the line items in CUR matches with the items in criteria, it will automatically proceeds with the cost allocation. The key part refers to the supported columns for matching the items. The value part refers to the specific value or regex of the items that may fall on that statement.
``` sh
  "criteria": [
    {
      "and": {
        "account": "re:^434343434343$",
        "description": "re:.*Fee.*",
        "productCode": "AWSSupportBusiness"
      }
    }
  ],
```

### Fee supported columns
``` sh
		"account",
		"lineType",
		"productCode",
		"description",
		"productName",
		"currency",
```

### Account Usage supported columns
``` sh
		"account",
		"operation",
		"description",
		"productCode",
		"usageType",
		"region",
```

### RI/SP Savings supported columns
``` sh
		"type",
		"payer",
		"account",
		"arn",
		"productCode",
		"offerClass",
```

`allocator` - Allocator controls the actual cost allocation. Each item on the allocator part determines how the allocation is to be done.
``` sh
  "allocator": [
    {
      "type": "account",
      "value": "434343434343",
      "formula": "cost*usage",
      "months": "3"
    },
    {
      "type": "account",
      "value": "123456789012",
      "formula": "cost*0.2",
      "months": "2"
    }
  ],
```

`Type` - This identifies the type of allocator. As of now, there are 3 types, "account", "payer" and "costgroup".

`Value` - This refers to the value of the type of allocator. If the type is set to account, the value is the account id. If the type is set to payer, the value is the payer id. If the type is set to costgroup, the value is the costGroupd id.

`Formula` - This refers how the cost allocation is to be computed. We have exposed 2 variables for the users, "cost" and "usage". Formula can also support conditional statements to determine its value.

`Cost` - This is the cost of the item to be allocated.

`Usage` -  This is the percentage of the usage of that account compared to all the listed accounts.

`Months` - This refers to the number of months the cost allocation is to be applied. If this is set to a number of months, the cost is to be divided equally to the number of months. If not set, the cost is applied to one month only.

### Simulate calculations
When creating a new allocator, there is an option to check if the allocator details can match with any existing data of the current month. This checks if the details are correct before saving the allocator details to the database. All possible results of the allocator are displayed.

## Allocations
All results after batch processing of fees and ri/sp savings are displayed under Allocations tab. Those are displayed according to the month they are generated.

Aside from the results of the batch processing, the results of the allocations are also displayed on the Allocations tab. 

### How this works?

1. Data are fetched from various sources and saved to database.
2. Available data on the database are processed using the Calculations items. Each item is checked if a criteria matches with the details of the items. If it matches, cost allocation proceeds.
3. From the list of allocators, all accounts are resolved. This means that if the type is costgroup and payer, all accounts from that group are listed. Monthly usage are fetched and corresponding formula are applied.
4. Allocation takes place with the usage and cost applied. Splitted items are stored in the database.
5. APIs to list all data for the specified month are called for UI display.

### Status

`Unchecked Allocated` - Original item is not distributed into cost allocations.

`Checked Allocated` - Cost allocation is applied on the original item.

`Checked Splitted` - The splitted item after cost allocation has been applied.

### Restoring a splitted item

Restore feature is only applied on the splitted items. This means that the cost allocation has been reverted. All cost allocations or distributions are removed and the original state has been loaded.

## Cost Groups Allocation Display

All available data after cost allocation are fetched and displayed under that costgroup.

**Account Usage**

Cost group computation is based on the existing costusage API. This means that whatever filters are applied on that cost group are applied also to the cost allocation items.

**Fees and RI/SP Savings**

Data are computed on a monthly basis. As of now, to get the available data for the cost group, all data are still based on the accounts in that cost group. 
