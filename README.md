### NEST3.0 Automatic quotation program operating instructions

[toc]


#### Introduction
>The NEST3.0 automatic quotation program is an example program. Related parameters such as block interval and quotation gasPrice multiples are not the optimal strategy plan. Users can adjust it according to the actual situation.

>Main functions：
   *  Check ETH and ERC20 balances
   *  ERC20 token approve
   *  Exchange price update（The example exchange is Huobi）
   *  Initiate a quotation 
   *  Cancel quotation (automatically determine whether the latest quotation block number of the contract has changed, if the latest block number is greater than the block number when you quote, then cancel the quotation to avoid being stuck in one block with other miner)。
   *  Retrieve staking assets


#### Preparation before starting

1. Get ready: wallet private key, Ethereum node URL, ERC20 token contract address, exchange trading pair API 

   * Wallet private key: Generated by mnemonic words, and can be registered through nestDapp. Quotation requires at least 10.2ETH and ERC20 tokens worth 10ETH
   * Ethereum node URL: You can apply for free via https://infura.io.
   * ERC20 token contract address: For example, if you quote USDT, you need to fill in the USDT token contract address 0xdac17f958d2ee523a2206206994597c13d831ec7
   * Trading pair settings: For example, if you chose ETH/USDT quotation , then the trading pair needs to fill in ethusdt. The Huobi trading pair query address is https://api.huobi.pro/v1/common/symbols


#### Startup and shutdown

1. Run the quotation program：
   * Double-click start.bat under the root path of the quotation program to run the quotation program, and a window will pop up. Please do not close it. You can view the take-order quotation and retrieve information in the window.

2. Log in：
   * Enter http://127.0.0.1:8088/offer/miningData in the browser, and you will enter the login page with the default user name nest and password nest.
   * If you need to modify the password, you can modify nest.user.name (user name) and nest.user.password (password) in Mining/src/main/resources/application.properties.

3. Close the quotation process:：
   * Stop mining before closing the quotation program, then wait 10 minutes, and close the window after the quotation asset is retrieved.

#### Settings

1. Ethereum node (required)：
   * The node address must be set first.

2. Set ERC20 address and Huobi trading pair (required):
   * For ETH/USDT quotes, fill in 0xdac17f958d2ee523a2206206994597c13d831ec7 for ERC20 address and ethusdt for Huobi trading pair.
   * For nToken quotation, please fill in the corresponding ERC20 address and Huobi trading pair.

3. Set the number of block intervals and multiples of gasPrice (not required):：
   * Can be adjusted according to the situation.

4. Set the agent address and port (not required)：
   * The agent address, which defaults to the native address of 127.0.0.1, if a network agent is used, the agent port must be configured.

5. Set private key (required)：
   * Fill in the private key, some initialization work will be carried out, such as mapping the address of each contract, obtaining the ERC20 token ID, the number of bits, etc. Please be patient

6. Start mining:
   * After the above configuration is completed, click the Modify Status button to modify the status of the miner to open. You can view the block height, the number of quotations, and the hash of the quotation transaction in the background (the window that pops up when you click start.bat).

#### Test quotation

```
1. Comment on the transaction code of the quotation:：
LOG.info("Quotation ETH quantity：{} Quote{}Quantity：{}  The amount of ETH transferred into the contract：{}", offerEthAmount, SYMBOL, offerErc20Amount, payableEth);
List<Type> typeList = Arrays.<Type>asList(
       new Uint256(offerEthAmount),
       new Uint256(new BigInteger(String.valueOf(offerErc20Amount))),
       new Address(ERC20_TOKEN_ADDRESS));
// Initiate transaction
String transactionHash = ethClient.offer(gasPrice, nonce, typeList, payableEth);

2. open http://127.0.0.1:8080/offer/miningData ,After the relevant configuration, modify the startup state。

3. Check the number of quoted ETH and quoted ERC20 in the printed log, and check the data
```



#### Contract interaction

[Contract interaction description](./Mining/README.md)
