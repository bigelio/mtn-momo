# mtn-momo
Node.js wrapper for the MTN Mobile Money API

## Usage
```javascript

// Import package
const momo = require('mtn-momo');

// (sandbox/development environment only) Provision/create a user and api key
const sandboxUserInfo = await momo.createApiUserAndKey({
  subscriptionKey: '<your-subscription-key>',
  providerCallbackHost: '<your-callback-host>'
});
const { userId, providerCallbackHost, targetEnvironment, apiKey } = sandboxUserInfo;

// Initialize the wrapper
const initializedWrapper = momo({
  apiKey: '<your-api-key>',
  userId: '<your-user-id>'
});
const { collections, disbursements, remittances } = initializedWrapper;



/* Collections API */

// (optional) Get an access token
const token = await collections.getToken();
const { access_token, token_type, expires_in } = token;

// Check if an account is active. Returns a boolean value
const isActive = await collections.isAccountActive({
  accountHolderIdType: 'MSISDN|EMAIL|PARTY_CODE',
  accountHolderId: '<account-holder-id>'
});

// Submist a request for payment
const paymentOptions = {
  amount: "string", // A number as a string
  currency: "string",
  externalId: "string",
  payer: {
    partyIdType: "MSISDN|EMAIL|PARTY_CODE",
    partyId: "string"
  },
  payerMessage: "string",
  payeeNote: "string"
};
const paymentId = await collections.initiate({
  callbackUrl: '<callback-url>',
  paymentOptions: paymentOptions
});

// Check the status of a request for payment
const status = await collections.fetchStatus(paymentId);
const {
  amount,
  currency,
  financialTransactionId,
  externalId,
  payer: {
    partyIdType,
    partyId
  },
  status: "SUCCESSFUL|FAILED|PENDING"
} = status;

// Check my account balance
const accountBalance = await collections.fetchAccountBalance();
const { availableBalance, currency } = accountBalance;

```
