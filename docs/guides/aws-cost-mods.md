# Adding cost modifiers to the AWS calculator using bluectl

!!! note
    This guide is only applicable to Ripple users.

Ripple allows the manipulation of cost values in its AWS cost calculations through [cost modifiers](https://alphauslabs.github.io/blueapidocs/#/Cost/Cost_CreateCalculatorCostModifier). Cost modifiers expose two variables, namely `usage` and `cost`, as inputs to user-provided formulas that can change the final cost during daily and monthly calculations. Since the modifier formula supports arithmetic, logical, and comparison operators, it is actually quite flexible.

In this guide, we will try and create cost modifiers to our AWS calculations. At the time of writing, there is no UI available yet for these APIs so we will use `bluectl` for the time being.

Make sure to install [`bluectl`](https://alphauslabs.github.io/docs/blueapi/bluectl/) first.

You might want to query the daily cost details first to know what kind of qualifiers (or filters) to use to further filter down your modifiers' targets. You can check out this [guide](https://alphauslabs.github.io/docs/guides/aws-query-costs/) for more information. For this guide though, we will use `productCode:awskms` and `operation:DescribeKey` under Tokyo region for our qualifiers.

!!! warning
    Ripple's trueunblended calculation uses a different logic than just referencing the lineitem's usage and cost. Be careful when applying modifiers to lineitems that are affected by trueunblended such as parts of AmazonEC2, AmazonRDS, AmazonElastiCache, AmazonES, and AmazonRedShift that are utilizing their respective RIs and/or SPs.

## Referencing usage
In this example, we will use a different rate of $0.005. Let's modify the description as well by enclosing it with an asterisk `*` so we will know later on what items were modified.

``` sh
# Let's use a file as our input:
$ cat /tmp/qualifier.json
{
  "awsOptions":{
    "groupId":"xWOGCzNy6GlK",
    "qualifiers":[
      {
        "and":{
          "productCode":"awskms",
          "region":"re:^ap.*-1$",
          "operation":"DescribeKey"
        }
      }
    ],
    "modifier":{
      "formula":"usage * 0.005",
      "descriptionModifier":{
        "prefix":"*",
        "suffix":"*"
      }
    }
  }
}

# Create the modifier:
$ bluectl cost aws calculator mods create \
  --raw-input "$(cat /tmp/qualifier.json)"

# Query our current modifiers:
$ bluectl cost aws calculator mods list --outfmt json --bare | jq
```

Unfortunately, we either have to wait for the calculator's next scheduled run or we request a billing group calculation to see the effects of these modifiers. We will provide an API to force-calculate specific targets without affecting invoice values in the future so stay tuned for that.

Once the calculation is completed, we can query the daily cost to check our results.

``` sh
$ bluectl cost aws usage get \
  --raw-input '{"groupId":"xWOGCzNy6GlK"}' \
  --out /tmp/out.csv
```

Open the CSV file, filter using our qualifiers, and confirm the resulting cost and the description columns.

## Referencing cost
Using the same set of commands above, in this example, we will modify the cost itself by adding a markup of 2%.

``` sh
$ cat /tmp/qualifier.json
{
  "awsOptions":{
    "groupId":"xWOGCzNy6GlK",
    "qualifiers":[
      {
        "and":{
          "productCode":"awskms",
          "region":"re:^ap.*-1$",
          "operation":"DescribeKey"
        }
      }
    ],
    "modifier":{
      "formula":"cost + (cost * 0.02)",
      "descriptionModifier":{
        "prefix":"*",
        "suffix":"*"
      }
    }
  }
}
```

## Referencing both usage and cost
The following examples refer only to the `formula` key in our `qualifier.json` file above.

This formula means that if the usage is less than 50, set the cost to 0, otherwise, return the current cost.

```
usage < 50.0 ? 0 : cost
```

This formula uses the following cost ranges:

* If cost is less than $10 or usage is less than 5, give 1% discount,
* If cost is $10 or more but less than $50, give 2% discount,
* If cost is $50 or more, give 3% discount.

(Should be one line)
```
cost < 10.0 || usage < 5.0
  ? cost - (cost * 0.01)
  : (cost >= 10.0 && cost < 50.0
       ? cost - (cost * 0.02)
       : cost - (cost * 0.03))
```
