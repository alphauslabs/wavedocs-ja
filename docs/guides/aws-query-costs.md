# Querying AWS costs using bluectl

[`bluectl`](https://alphauslabs.github.io/docs/blueapi/bluectl/) is a flexible tool that you can use to query your cost data in Ripple/Wave(Pro). Since it also supports most, if not all of the possible API parameters, in some cases, you can even query data combinations that are not yet available in our UI.

!!! info "--raw-input"
    [Blue API](https://alphauslabs.github.io/blueapidocs/) and [`bluectl`](https://alphauslabs.github.io/docs/blueapi/bluectl/) are still in beta. For `bluectl` to be able to support most Blue API parameter combinations, it provides a `--raw-input` flag which accepts the same JSON input required in Blue API for most of its commands. Refer to the examples using the `--raw-input` flag below.

    As Blue API becomes more stable, we will provide more easy-to-use parameters to `bluectl` so you don't have to use the `--raw-input` flag in the future. Stay tuned.

## Querying a billing group's daily costs for the current month
``` sh
# Replace 'abcdef' with your billing internal id.
$ bluectl cost aws get --raw-input '{"groupId":"abcdef"}'
```

## Querying an account's daily costs for the current month
``` sh
# Replace '012345678901' with your account id.
$ bluectl cost aws get --raw-input '{"accountId":"012345678901"}'
```

## Using date ranges in your query
``` sh
# Query costs for the month of December, 2021.
$ bluectl cost aws get --raw-input \
  '{"accountId":"012345678901","startTime":"20211201","endTime":"20211231"}'
```

## Exporting your queries to CSV
You can use the `--out {location}` flag to export your queries to a CSV file. For example:
``` sh
$ bluectl cost aws get --raw-input '{"groupId":"abcdef"}' --out /tmp/out.csv
```

!!! info "Reference"
    Check out the JSON request format [here](https://alphauslabs.github.io/blueapidocs/#/Cost/Cost_ReadCosts) to know more about the `bluectl cost aws get --raw-input '{...}'` command's supported parameters.

!!! tip "Tip: use file as --raw-input"
    If your `--raw-input` is getting longer and is becoming difficult to read, you can write it in a file, and then reference that file in your command. For example,
    ``` sh
    # This is your query file. Modify as needed.
    $ cat /tmp/query.json
    {
      "accountId":"012345678901",
      "startTime":"20211201",
      "endTime":"20211231"
    }
    
    # Use query.json file as your --raw-input.
    $ bluectl cost aws get --raw-input "$(cat /tmp/query.json)" --out /tmp/out.csv
    ```

## Grouping by month
``` sh
# Query monthly service costs from December, 2021 to January 2022.
$ cat /tmp/query.json
{
  "accountId":"012345678901",
  "startTime":"20211201",
  "endTime":"20220131",
  "awsOptions":{
    "groupByMonth":true
  }
}

$ bluectl cost aws get --raw-input "$(cat /tmp/query.json)" --out /tmp/out.csv
```

## Grouping by cost attributes
You can group your costs by cost attributes (or columns). Blue API supports the following attributes for grouping: `productCode`, `serviceCode`, `region`, `zone`, `usageType`, `instanceType`, `operation`, `invoiceId`, `description`, and `resourceId`. You can group by any combination of these attributes.

``` sh
# Query January 2022 daily costs grouped by service and region.
$ cat /tmp/query.json
{
  "accountId":"012345678901",
  "startTime":"20220101",
  "endTime":"20220131",
  "awsOptions":{
    "groupByColumns":"productCode,region"
  }
}

$ bluectl cost aws get --raw-input "$(cat /tmp/query.json)" --out /tmp/out.csv
```

``` sh
# Query January 2022 costs grouped by service and region, then group by month.
$ cat /tmp/query.json
{
  "accountId":"012345678901",
  "startTime":"20220101",
  "endTime":"20220131",
  "awsOptions":{
    "groupByColumns":"productCode,region",
    "groupByMonth":true
  }
}

$ bluectl cost aws get --raw-input "$(cat /tmp/query.json)" --out /tmp/out.csv
```

## Filtering your queries
Blue API provides a flexible way for you to further filter your queries in combination with your cost attributes. This allows you to query more specific information such as specific services, or specific regions, or a combination of the two.

``` sh
$ cat /tmp/query.json
{
  "accountId":"012345678901",
  "startTime":"20220101",
  "endTime":"20220131",
  "awsOptions":{
    "filters":[
      {
        "andFilters":{
          "productCode":"AmazonEC2",
          "region":"ap-northeast-1"
        }
      },
      {
        "andFilters":{
          "productCode":"AmazonRDS"
        }
      }
    ]
  }
}

$ bluectl cost aws get --raw-input "$(cat /tmp/query.json)" --out /tmp/out.csv
```

The example above can be interpreted as:

* (`productCode` is equal to "AmazonEC2" **AND** `region` is equal to "ap-northeast-1") **OR**
* (`productCode` is equal to "AmazonRDS")

!!! info ""
    The items inside the `andFilters` key use the AND operator, while the `andFilters` themselves use the OR operator.

Blue API filters also support regular expressions using Google's [RE2](https://github.com/google/re2/wiki/Syntax) by prefixing your filter values with either `re:` (match) or `!re:` (reverse match).

``` sh
# Query all EC2 or RDS costs in all regions starting with "ap-".
$ cat /tmp/query.json
{
  "accountId":"012345678901",
  "startTime":"20220101",
  "endTime":"20220131",
  "awsOptions":{
    "filters":[
      {
        "andFilters":{
          "productCode":"re:AmazonEC2|AmazonRDS",
          "region":"re:^ap-.*"
        }
      }
    ]
  }
}

$ bluectl cost aws get --raw-input "$(cat /tmp/query.json)" --out /tmp/out.csv
```

## Filtering by tags
Tag filtering is similar to filtering by attributes in terms of the logic order and regular expressions support. The only difference is that your `andFilters` map will contain the tag key/values.

``` sh
# Query all costs that are tagged with 'key=user:Name' and 'value=some-name'.
$ cat /tmp/query.json
{
  "accountId":"012345678901",
  "startTime":"20220101",
  "endTime":"20220131",
  "awsOptions":{
    "tagFilters":[
      {
        "andFilters":{
          "user:Name":"some-name"
        }
      }
    ]
  }
}

$ bluectl cost aws get --raw-input "$(cat /tmp/query.json)" --out /tmp/out.csv
```
