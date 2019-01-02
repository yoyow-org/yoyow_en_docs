# User Registration

## Registering Accounts
The account of the YOYOW platform is a string of numbers (similar to the QQ number). It is assigned by the system when registering, and the nickname function will be released in the future.

Currently, accounts can only be registered through [Web Wallet](https://wallet.yoyow.org) or [Client Wallet](https://yoyow.org/client1.html).
You can refer to: [Web Wallet Registration Method](https://steemit.com/cn/@peterchen145/yoyow-online-wallet-sign-up-tutorial)

Special attention is needed here. The system will generate a private Owner Key when registering. The private key only appears once upon successful registration. Please make sure to back up and save it properly since the web wallet and client wallet will not store your private Owner Key. 

## Private Key Authority

In the YOYOW system, there are four types of private keys for each account.
- Owner Key: it has the highest authority. It is the priavate key that can modify other authorities of the account. The private key that appears upon successful registration is the Owner Key.
- Active Key: it has the authority to use the assets in the balance, such as controlling the actions of the transfer.
- Secondary Key: it has the authority to use the tipping, including the actions of authorizing the platforms. It is also called tipping Key.
- Memo Key: it is used to check the memo information of the transfer actions.

Authorization at each level is for actions within that level only. No action beyond or below one's authorization is allowed. Active Key does not have the right to tranfer tipping, and Secondary Key does not have the right to transfer balances either. The private Owner Key does not have the authority to use balances and tipping either, but it can reset the private keys of other authority levels. Other three types of private keys can be reset by importing the Owner Key of the account in the web wallet.

For more info about private keys, you can refer to [yoyow private key tutorials](https://steemit.com/cn/@peterchen145/yoyow-keys-tutorial-yoyow)

## Private Key Saving

Private Owner Key only appears once upon successful registration. The browser will not save the private key, and neither will YOYOW. It is recommended to save your private Owner Key properly and safely and ensure that it is free from being lost and stolen. It is dangerous to store the private key in plaintext on netdisk or online notes. It is also recommended not to use private Owner Key easily.

In the web wallet, other private keys from lower authority levels will be encrypted using your wallet password and stored in the browser's local database. The same is true for the client wallet. Your account can be rebuilt using the private Owner Key while certain circumstances (such as switching computer, computer failure or browser crash) lead to the loss of local data on your browser.
