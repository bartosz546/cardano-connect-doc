<div align="center">
  <img src="https://dapp-connects.com/assets/images/cardano-connect-preview.jpg" width="500" alt="Angular Toastr">
  <br>
  <br>
  <br>
</div>

DEMO: https://dapp-connects.com/

JS FIDDLE: https://jsfiddle.net/bartosz546/3rt240wb/

## Setup

**step 1:** add Dapp Connects script the head of your DApp

```javascript
  <script src="https://dapp-connects.com/assets/cardano-connect/scripts/cardano-connect-client@latest.js"></script>
```

**step 2:** Initialize CardanoConnect instance

```javascript

let cardanoConnectInstance = new CardanoConnect();

cardanoConnectInstance.setOnWidgetLoadedListener(() => {
  cardanoConnectInstance.connectWallet(
    {
      requestApiProof: true,
      showErrorModal: true,
    }
  ).then(async (connectionResponse) => {
    const response = await verifyUserSignature(connectionResponse.apiProof);
    console.log("Signature valid: ", response.data);
    this.walletConnected = true;
  }).catch(((rej) => { console.log(rej, rej.message); }));
});
```

**step 3:** Verify signature authenticity (ON THE SERVER)

```javascript
// API call to dapp connects verifying signature authenticity
// Implement on the server side to handle authentication and authorize the user accordingly
// Use stake address like user ID
let verifyUserSignature = async (apiProof) => {
  return await fetch("https://connect.dapp-connects.com/authorization/verify-signature", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(apiProof)
  }).then(response => response.json());
}

const response = await verifyUserSignature(apiProof);
console.log("Signature valid: ", response.data);

```

## Options

### connectWallet(options)

| Option                             | Type                           | Default | Description                                                                                   
|------------------------------------|--------------------------------|--------|-----------------------------------------------------------------------------------------------|
| requestApiProof                    | boolean                        | false  | Request Wallet to Sign Random Generated Data String to verify Wallet Owner on the server side |
| showErrorModal                     | boolean                        | false  | Show close button                                                                             |
| rejectOnError                      | boolean                        | false  | Reject promise returned and close connection modal                                            |
| onModalOpened                      | Function                       |        | On modal opened and modal html elements created                                               |
| onErrorModalOpened                 | Function                       |        | On error modal opened and modal html elements created                                         |
| walletConnectionInProgressListener | Function                       |        | Callback informing, if wallet connection is in progress                                       |
| walletConnectionErrorsListener     | Function                       |        | Callback informing, on errors user experiences                                                |
| supportEmailAddress                | string                         | ""     | Support email address which gonna show up on error modal for your users                       |

### requestAdaTransfer(address, lovelaceAmount, showErrorModal, lucidAddress)

| Option                              | Type     | Default                                            | Description                                                                                                         
|-------------------------------------|----------|----------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| address                             | string   | false                                              | Address you want to send ADA to                                                                                     |
| lovelaceAmount                      | number   |                                                    | Amount of ADA in lovelace you want to transfer                                                                      |
| showErrorModal                      | boolean  | false                                              | Show error modal, on transaction rejection                                                                          |
| lucidAddress                        | Function | https://unpkg.com/lucid-cardano@0.10.11/web/mod.js | We dynamically load Lucid library to request transfer, if you have it locally you can replace it with your instance |

### getCip30Api(options)
Get getCip30 wallet Api. 
Options same as for `connectWallet`.

### getAllWallets()

Get all available in widget wallets. 

### getConnectedWallet()

Get currently enabled wallet. 

### disconnectWallet()

Disconnect wallet. 

### closeConnectWalletModal()

Close connect wallet modal and reject its promise. 


## localStorage

We keep information on currently connected wallet in localStorage keep in mind to not blindly clear it.




---

> GitHub [@bartosz546](https://github.com/bartosz546) &nbsp;&middot;&nbsp;
