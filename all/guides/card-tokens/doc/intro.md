SortOrder: 0
### About the Card Tokens API:
Used to tokenize credit or debit card data in PCI DSS environment. 
This card token must be passed to [CreateTransaction](/doc/needs link to endpoint) when a credit/debit card is the chosen payment method.
> **Note:** A new `card_token` should be generated for each transaction request.
> If you are creating a recurring transaction (MIT/CIT) you only need to tokenize the `card_number` and pass it in the [CreateTransaction](/doc/needs link to endpoint) request together with [COF](/doc/link needed) flags.

According to the PCI DSS guidelines, services are not allowed to store credit card information 
outside of the secured PCI environment. 
A token refers to a credit card number and security code. 
Tokenization is the process of protecting sensitive data by replacing it with an algorithmically 
generated number called a token. Tokenization is used to prevent credit card fraud. 
The following functionality is provided to tokenize the card information in compliance to the PCI DSS.
