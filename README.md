# Aptos Creed - Web 3.0 Blockchain Application
![Aptoscreed](https://github.com/Thycrescendo/Aptos-Creed/blob/main/client/aptt.png)

Build, Sign, and Broadcast with Aptos
For developers eager to explore the benefits of the Aptos blockchain, Validation Cloud's Aptos API offers a powerful and seamless entry point.
To get started, our comprehensive documentation provides a clear roadmap for setting up your development environment and connecting to the Aptos Network. In the next section, we'll walk you through setup, querying the chain, importing your key to an Aptos account, simulating a transaction, and then sending a transaction. First, sign up for free at validationcloud.io, and create your Aptos API key.

SetupLet's get your TypeScript environment prepared. Ensure you have Node.js and Yarn installed, and then set up a new TypeScript project:
mkdir aptos-project && cd aptos-project
yarn init -y
yarn add axios dotenv typescript ts-node @types/node @aptos-labs/aptos @aptos-labs/ts-sdk

Create a '.env' file in your project root to store your API key from app.validationcloud.io.
The ts-sdk version for this tutorial is ^1.13. Additionally, we will store a pre-funded wallet's private key here as well. Storing private keys in plaintext is strongly not advised for wallets containing any sort of significant balance. In those scenarios, a more secure solution such as AWS Secrets Manager, Azure Key Vault, HashiCorp Vault, or others would be expected.
API_KEY="https://mainnet.aptos.validationcloud.io/v1/<YOUR_API_KEY_HERE>"
PRIV_KEY="Your_PrivateKey_Here" 
MY_ADDRESS=0xbd2......
FRIEND_ADDRESS=0x06d0.....

1. Connecting to Aptos, and Retrieving Ledger Info
Let's start by connecting to Aptos and fetching some ledger info. The returned values will include the chainID, epoch, ledger version, timestamp, the git hash, and the blockheight, among a couple other properties.
import axios from 'axios';
require('dotenv').config();
const API_URL = process.env.API_KEY;
async function getLedgerInfo() {
    try {
      const ledger_info = await axios.get(`${API_URL}/v1`);
      console.log('Ledger Info:', ledger_info.data);
      return ledger_info.data;
    } catch (error) {
      console.error('Error fetching the ledger:', error);
    }
  }
  
getLedgerInfo();

2. Get the Latest Block by Height
For this method call, we'll build off of the information from #1 to grab the latest block, and then run another call to get that latest block's details.
async function getLatestBlock() {
    try {
      const ledger_info = await axios.get(`${API_URL}/v1`);
      const latest_block = ledger_info.data.block_height
      const block_info = await axios.get(`${API_URL}/v1/blocks/by_height/${latest_block}`)
      console.log('Latest Block:', block_info.data);
      return block_info.data;
    } catch (error) {
      console.error('Error fetching the latest block:', error);
    }
  }
  
getLatestBlock();

3. Get an Account by Address
Retrieving an account by its address on the Aptos blockchain, even if it only provides basic information such as the authentication key and sequence number, serves several practical purposes. The authentication key is crucial for confirming the identity and the cryptographic credentials associated with the account, which is fundamental for security and transaction verification processes. This key helps ensure that any transaction submitted is genuinely from the account holder. The sequence number of an account, on the other hand, is vital for transaction ordering. Each transaction from an account must have a sequence number greater than the last confirmed transaction.
async function getAccountByAddress() {
    const accountAddress = '0xe6c6288b3f88936ba7e951b9f8e21d50972a6671e1f97d3cb9346fbf5013a289';
    try {
      const resource = await axios.get(`${API_URL}/v1/accounts/${accountAddress}`);
      console.log("Account Info:", resource.data);
    } catch (error) {
      console.error("Error fetching account resource:")//, error);
    }
  }
  
getAccountByAddress();

4. Get an Account Resource
An example of an account resource on Aptos could be the coin balance resource.
For instance, the 0x1::coin::CoinStore<0x1::aptos_coin::AptosCoin> resource defines the balance of Aptos Coins (the native currency) held in an account, including methods for managing these funds like depositing and withdrawing. Fetching an account resource on the Aptos blockchain provides valuable insights into the state of a specific account. This operation is crucial for developers and users wanting to understand the properties and current state of an account, such as its balances, sequence numbers, and associated modules and resources.
Note, getting the coin resource defaults to the unit of "microAptos," so we convert it to APT for readability below.
async function fetchAptosCoinBalance(address: string) {
    const resourceType = '0x1::coin::CoinStore<0x1::aptos_coin::AptosCoin>';
    try {
        const url = `${API_URL}/v1/accounts/${address}/resource/${encodeURIComponent(resourceType)}`;
        const response = await axios.get(url);
        const microAptos = response.data.data.coin.value;
        const aptos = microAptos / 1000000;  // Convert microAptos to APT
        console.log(`Balance for ${address.slice(0,10)+"...."}: ${aptos} APT`);
        return { microAptos, aptos };
    } catch (error) {
        console.error('Error fetching coin balance:', error);
        throw new Error('Failed to fetch coin balance');
    }
}
const address = '0xe6c6288b3f88936ba7e951b9f8e21d50972a6671e1f97d3cb9346fbf5013a289';
fetchAptosCoinBalance(address);

5. Simulating a Transaction
If you've ever made a mistake issuing a transaction on Web3 before and wished you could have known the mistake in advance, you're not alone!
Simulating a transaction on Aptos is a powerful tool for developers and users to predict and analyze the outcome of a transaction without committing it to the blockchain. This simulation reveals whether a transaction will succeed or fail, and provides details about the changes it would make if committed, such as gas costs and state mutations. This is invaluable for debugging and optimizing smart contracts and transactions before they go live, which can save time and reduce costs by preventing failed transactions and their associated fees. Moreover, simulation helps in stress-testing smart contracts under various conditions to ensure their robustness and security, enhancing trust in the applications built on top of the Aptos platform.
We will now be using the Aptos official TS SDK (which contains utilities for transaction signing and broadcasting) to simulate sending 0.01 APT from one account to another.
import { 
    Account, AccountAddress, 
    Aptos, AptosConfig, 
    Ed25519PrivateKey, Network } 
    from "@aptos-labs/ts-sdk";
import * as dotenv from "dotenv";
dotenv.config();
// Constants
const APTOS_COIN = "0x1::aptos_coin::AptosCoin";
const API_URL = process.env.API_URL; // The URL of the Validation Cloud API endpoint
const privateKeyHex = process.env.PRIV_KEY; // The private key from your .env file
const myAddress=process.env.MY_ADDRESS;
const friendAddress=process.env.FRIEND_ADDRESS
console.log("PRIVATEKYEYHEX",privateKeyHex);
if (!privateKeyHex || !myAddress || !friendAddress) {
    throw new Error("Ensure required variables are set in the environment file.");
}
// Convert privateKeyHex to Bytes
const formattedPrivateKeyHex = privateKeyHex.startsWith('0x') ? privateKeyHex.substring(2) : privateKeyHex;
const privateKeyBytes = new Uint8Array(Buffer.from(formattedPrivateKeyHex, 'hex'));
const privateKey = new Ed25519PrivateKey(privateKeyBytes);
const accountAddress = AccountAddress.from(myAddress);
// Importing the Aptos account from exported private key 
const myAccount = Account.fromPrivateKey({
  privateKey,
  address: accountAddress,
  legacy: true, // Exported from Martian Wallet, so legacy is required
});
// Initialize Aptos Client with configuration
const config = new AptosConfig({
    fullnode: API_URL,
    network: Network.MAINNET,
});
const aptos = new Aptos(new AptosConfig(config))
async function simulateTransaction(aptos: Aptos, myAccount: Account, friendAddress: string) {
    try {
        // Build the transaction for transferring 0.01 APT
        const transaction = await aptos.transaction.build.simple({
            sender: myAccount.accountAddress,
            data: {
                function: "0x1::coin::transfer",
                typeArguments: [APTOS_COIN],
                functionArguments: [friendAddress, 1000000], // "Octa" APT   
            },      
        });
        // Simulate the transaction
        const [userTransactionResponse] = await aptos.transaction.simulate.simple({
            signerPublicKey: myAccount.publicKey,
            transaction,
        });
        // Print the response from the simulation
        console.log("Simulation response:", userTransactionResponse);
    } catch (error) {
        // Catch and print any errors encountered during the transaction build or simulation
        console.error("Error simulating the transaction:", error);
    }
}
simulateTransaction(aptos, myAccount, friendAddress)

Note: Before issuing tokens to any Aptos account via the SDK, the account must have registered to receive tokens first. Creating a CoinStore resource on an account for storing coins will resolve any errors encountered here, alternatively, a wallet extension can be used to send APT tokens to the account in question, creating the coin resource on the account automatically.
Note 2: The private key exported for these examples was from the Martian wallet. As of writing, the key format exported was legacy, and Ed25519 format.
6. Sending a Transaction
Keeping everything above the function in the prior example the same, the following code essentially alters the transaction.simulate to a transaction.submit, now calling the appropriate client resources to commit the transaction to the blockchain itself.
async function sendTransaction(aptos: Aptos, myAccount: Account, friendAddress: string) {
    try {
        // Build the transaction for transferring 0.01 APT
        const transaction = await aptos.transaction.build.simple({
            sender: myAccount.accountAddress,
            data: {
                function: "0x1::coin::transfer",
                typeArguments: [APTOS_COIN],
                functionArguments: [friendAddress, 1000000] // in "Octa" APT = 0.01 APT 
            },
        });
        // using sign and submit separately
        const senderAuthenticator = aptos.transaction.sign({
            signer: myAccount,
            transaction,
        });
        console.log("TRANSACTION SIGNED")
        const committedTransaction = await aptos.transaction.submit.simple({
            transaction,
            senderAuthenticator,
          });
        console.log("Transaction Committed:", committedTransaction);
    } catch (error) {
        // Catch and print any errors encountered during the transaction build or simulation
        console.error("Error issuing the transaction:", error);
    }
}
sendTransaction(aptos, myAccount, friendAddress)

Through these examples, you've learned how to effectively interact with the Aptos blockchain using TypeScript and Axios. This tutorial should serve as a foundation for building more complex applications on top of the Aptos platform. Stay tuned for future posts where we'll dive deeper into smart contract interactions and node API possibilities.

About Validation CloudValidation Cloud is a Web3 data streaming and infrastructure company that connects organizations into Web3 through a fast, scalable, and intelligent platform. Headquartered in Zug, Switzerland, Validation Cloud offers highly performant and customizable products in staking, node, and data-as-a-service.
