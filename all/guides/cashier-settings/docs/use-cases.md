SortOrder: 1
## Use Cases
Begin using 3rd party payments with your Wix integration in just three steps:
1. Spot the best payment methods and providers in the desired country.
2. Set up payment provider accounts to sell with Wix via chosen payment methods.
3. Manage checkout and periodically check statuses of your provider accounts to ensure the continuity of your sales.

### Spot the best payment methods and providers in the desired country.

#### Check current marchant information
Ensure the correctness of Country and eligibility to sell with Wix.
```
curl 'www.wixapis.com/cashier/v2/settings/merchant' -H 'Content-Type: application/json' -H 'Authorization: XXX'
```

```json
{
  "merchant": {
    "country": "GBR",
    "paymentsAllowed": true,
    "email": "merchant@example.com"
  }
}
```

####  Get list of Payment Methods available in country for Merchant
Check “recommendations” attribute, where consumer can find:
- Is this Payment Method recommended 
- Available Payment providers that provide each Payment method (sorted by recommendation)
```
curl 'www.wixapis.com/cashier//v2/settings/payment-method-types' \
   -H 'Content-Type: application/json' \
   -H 'Authorization: <AUTH>'
```

```json
{
  "methodTypes": [
    {
      "id": "creditCard",
      "icon": {
        "url": "https://images-wixmp-6613fa290e8c1ac70a0297b6.wixmp.com/payment-methods/onboarding/svg/creditCard.svg"
      },
      "recommendations": {
        "promoted": true,
        "paymentProviderIds": [
          "com.wix.wixpay.us",
          "com.stripe",
          "com.square",
          "com.worldpay.smb",
          "com.braintreegateway",
          "sumUp"
        ]
      }
    },
    {
      "id": "payPal",
      "icon": {
        "url": "https://images-wixmp-6613fa290e8c1ac70a0297b6.wixmp.com/payment-methods/onboarding/svg/payPal.svg"
      },
      "recommendations": {
        "promoted": true,
        "paymentProviderIds": [
          "payPal"
        ]
      }
    },
    {
      "id": "cash",
      "icon": {
        "url": "https://images-wixmp-6613fa290e8c1ac70a0297b6.wixmp.com/payment-methods/onboarding/svg/cash.svg"
      },
      "recommendations": {
        "paymentProviderIds": [
          "NA"
        ]
      }
    },
    ...
  ]
}
```

#### Get list of Payment Providers available in country for Merchant
```
curl 'www.wixapis.com/cashier/v2/settings/payment-providers' \
   -H 'Content-Type: application/json' \
   -H 'Authorization: <AUTH>'
```

```json
{
  "providers": [
    {
      "id": "payPal",
      "title": "PayPal",
      "paymentMethodTypeInfos": [
        {
          "paymentMethodTypeId": "payPal",
          "fee": {
            "amount": {
              "value": "0.2",
              "currency": "GBP"
            },
            "percent": "2.9"
          },
          "supportRecurring": true,
          "brands": [
            {
              "title": "PayPal",
              "icon": {
                "url": "https://images-wixmp-6613fa290e8c1ac70a0297b6.wixmp.com/payment-brands/svg/brand-paypal.svg"
              }
            }
          ]
        },
        {
          "paymentMethodTypeId": "venmo"
        },
        {
          "paymentMethodTypeId": "payPalCredit"
        }
      ],
      "onboarding": {
        "oauthConnect": {},
        "oauthCreate": {},
        "oauthClaim": {}
      },
      "links": {
        "wixHelpUrl": "https://support.wix.com/en/article/connecting-paypal-as-a-payment-provider",
        "providerHelpUrl": "https://www.paypal.com/uk/selfhelp/home"
      },
      "icon": {
        "url": "https://images-wixmp-6613fa290e8c1ac70a0297b6.wixmp.com/payment-providers/svg/payPal.svg"
      }
    },
    ...
  ]
}
```

### Set up payment provider accounts to sell with Wix via chosen payment methods.

Workflow:
1. Choose desired combination of Payment method and Payment provider as described above.
2. Call Get Payment Providers with the desired Payment provider and check the “onboarding” attribute that defines the way of establishing the connection.
    - For direct type of connection - call Connect Provider Account Directly with credentials that are issued by the Payment provider
    - For oauth type of connection - call Connect Provider Account OAuth and follow the link that is returned in the response to finalize the connection. After all, consumer will be returned back to the returnUrl that was passed in initial call
3. Listen to Provider Account Created Domain Event. Payment Provider Account object will indicate status of the connection to the Payment provider, ability to receive payments and payouts, and required actions (if any).

#### Add Payment Provider Account with Direct Connection
Send credentials as required in onboarding of Payment Provider metadata.
```
curl -X POST \
   'www.wixapis.com/cashier/v2/settings/provider-accounts/direct-connect'' \
    --data-binary '{
                     "paymentProviderId": "239e7846-6f6c-4801-86bc-d14b9151c534",
                     "credentials": {
                       "username": "merchant@example.com ",
                       "password": "xxxxxxxxxxxxxxxx"
                     },
                     "installments": null,
                     "paymentMethodTypeIds": [
                       "creditCard"
                     ]
                   }' \
   -H 'Content-Type: application/json' \
   -H 'Authorization: <AUTH>'
```

```json
{
    "providerAccount": {
      "id": "239e7846-6f6c-4801-86bc-d14b9151c534",
      "paymentProviderId": "239e7846-6f6c-4801-86bc-d14b9151c534",
      "payinsAllowed": true,
      "payoutsAllowed": true,
      "status": "ACTIVE",
      "name": "Test******",
      "additional": {}
    }
}
```

#### Add Payment Provider with OAuth Connection
```
curl -X POST \
   'www.wixapis.com/cashier/v2/settings/provider-accounts/oauth-connect' \
    --data-binary '{
                     "paymentMethodTypeIds": ["creditCard"],
                     "paymentProviderId": "com.stripe",
                     "returnUrl": "https://www.wix.com/dashboard/2xxxx-28f3-4154-b27c-xxxxxx/payments/methods"
                   }' \
   -H 'Content-Type: application/json' \
   -H 'Authorization: <AUTH>'
```

```json
{
  "redirectUrl": "/_api/payment-services-web/merchant/stripe/connect/start/login?appDefId=14bca956-e09f-f4d6-14d7-466cb3f09103&appInstanceId=66056ce8-c750-4e00-ae43-0313d26ebdb1&paymentMethod=creditCard&returnUrl=https://www.wix.com/dashboard/2xxxx-28f3-4154-b27c-xxxxxx/payments/methods"
}
```

In a case of error during oAuth flow such error is reported as "errorCode" GET-parameter, added to return_url in a case of error.
Possible values:
- `29` Generic connect complete error. Merchant just declined to proceed oAuth connect flow. 
- `30` Authorization grant declined by Merchant.

#### Create payment method for added Payment Provider Account
```
curl -X POST \
   'www.wixapis.com/cashier/v2/settings/provider-accounts/com.stripe/payment-method-type-setup' \
    --data-binary '{
                     "paymentMethodTypeId": "creditCard",
                     "paymentMethodTypeSetup": {
                       "instructions": null,
                       "enabled": true,
                       "paymentMethodTypeId": "creditCard"
                     },
                     "providerAccountId": "239e7846-6f6c-4801-86bc-d14b9151c534"
                   }' \
   -H 'Content-Type: application/json' \
   -H 'Authorization: <AUTH>'
```

```json
{
  "providerAccount": {
    "id": "239e7846-6f6c-4801-86bc-d14b9151c534",
    "paymentProviderId": "239e7846-6f6c-4801-86bc-d14b9151c534",
    "payinsAllowed": true,
    "payoutsAllowed": true,
    "status": "ACTIVE",
    "name": "Test******",
    "additional": {},
    "paymentMethodTypeSetups": [
      {
        "paymentMethodTypeId": "creditCard",
      }
    ]
  }
}
```

### Enable/disable Payment methods at the checkout
Call **Update Provider Account Payment Method** and pass “enabled” property for desired Payment method
```
curl -X POST \
   'www.wixapis.com/cashier/v2/settings/provider-accounts/com.stripe/payment-method-type-setup' \
    --data-binary '{
                     "paymentMethodTypeId": "creditCard",
                     "paymentMethodTypeSetup": {
                       "instructions": null,
                       "enabled": true,
                       "paymentMethodTypeId": "creditCard"
                     },
                     "providerAccountId": "239e7846-6f6c-4801-86bc-d14b9151c534"
                   }' \
   -H 'Content-Type: application/json' \
   -H 'Authorization: <AUTH>'
```

```json
{
  "providerAccount": {
    "id": "239e7846-6f6c-4801-86bc-d14b9151c534",
    "paymentProviderId": "239e7846-6f6c-4801-86bc-d14b9151c534",
    "payinsAllowed": true,
    "payoutsAllowed": true,
    "status": "ACTIVE",
    "name": "Test******",
    "additional": {},
    "paymentMethodTypeSetups": [
      {
        "paymentMethodTypeId": "creditCard",
        "enabled": true
      }
    ]
  }
}
```

### Periodically check status of added Provider Accounts to ensure the continuity of the sales

Periodically call **List Provider Accounts** and monitor the status of Provider Accounts as well as:
- Payments status
- Payouts status
- Required Actions

```
curl -X POST \
   'www.wixapis.com/cashier/v2/settings/provider-accounts' \
   -H 'Content-Type: application/json' \
   -H 'Authorization: <AUTH>'
```

```json
{
  "providerAccounts": [
    {
      "id": "com.stripe",
      "paymentProviderId": "com.stripe",
      "payinsAllowed": true,
      "payoutsAllowed": true,
      "status": "ACTIVE",
      "name": "Stri******",
      "additional": {
        "data": {
          "publicKey": "Stripe Sandbox",
          "accountName": "Stripe Sandbox"
        }
      }
    },
    {
      "id": "NA",
      "paymentProviderId": "NA",
      "payinsAllowed": true,
      "payoutsAllowed": true,
      "status": "ACTIVE",
      "additional": {}
    },
    {
      "id": "com.worldpay.smb",
      "paymentProviderId": "com.worldpay.smb",
      "payinsAllowed": true,
      "payoutsAllowed": true,
      "status": "ACTIVE",
      "name": "T_S_******",
      "additional": {}
    },
    {
      "id": "239e7846-6f6c-4801-86bc-d14b9151c534",
      "paymentProviderId": "239e7846-6f6c-4801-86bc-d14b9151c534",
      "payinsAllowed": true,
      "payoutsAllowed": true,
      "status": "ACTIVE",
      "name": "Test******",
      "additional": {}
    },
    {
      "id": "com.wix.wixpay.us",
      "paymentProviderId": "com.wix.wixpay.us",
      "payinsAllowed": true,
      "status": "ACCEPTING_PAYMENTS",
      "requiredAction": "TO_BE_CLAIMED_GOT_PAYMENT",
      "name": "dc23******",
      "country": "US",
      "additional": {
        "data": {
          "wixPayStatus": "Pending"
        }
      }
    },
    {
      "id": "payPal",
      "paymentProviderId": "payPal",
      "payinsAllowed": true,
      "payoutsAllowed": true,
      "status": "ACTIVE",
      "name": "test******",
      "additional": {
        "data": {
          "permissionsGranted": "false"
        }
      }
    }
  ]
}
```
