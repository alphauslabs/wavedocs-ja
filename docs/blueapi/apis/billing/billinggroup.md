# Billing group documentation

Description of values related to Billing group.

## Guidance

Definition of Billing group.

```
message BillingGroup {
  string billingInternalId = 1;

  string billingGroupId = 2;

  string billingGroupName = 3;

  string companyName = 7;
}
```

### billingInternalId

The following section describes the use of the `billingInternalId`.

This is a unique ID given when creating a billing group. This value cannot be changed.

### billingGroupId

The following section describes the use of the `billingGroupId`.

This is a ID for users to manage billing groups in products(Ripple/Wave[Pro]).

### billingGroupName

The following section describes the use of the `billingGroupName`.

This is a Name for users to manage billing groups in products(Ripple/Wave[Pro]).

### companyName

The following section describes the use of the `companyName`.

This is a Name for users to manage company involved in billing.