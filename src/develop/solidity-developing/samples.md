---
sidebar_position: 2
---

# Solidity Contracts Examples

This set of smart-contract samples illustrates standard functionality of smart-contracts developed in Solidity, starting with very basic and gradually evolving into code snippets, which may come handy in production smart-contracts.

- You can find a list of all the contracts listed in this EverX (formerly TON Labs) [repository](https://github.com/tonlabs/samples/blob/master/solidity/).

### 1. [Accumulator](https://github.com/tonlabs/samples/blob/master/solidity/1_Accumulator.sol) : persistent storage

Smart-contracts deployed to the blockchain store their state variables in a persistent storage.
Call `Accumulator.add(uint value)`. It adds `value` to its state variable `sum`.
Resulting state of the account can be examined by conventional means.

### 2. [StorageClient](https://github.com/tonlabs/samples/blob/master/solidity/2_StorageClient.sol): calling another [contract](https://github.com/tonlabs/samples/blob/master/solidity/2_UintStorage.sol)

Contracts can also call other remote contracts. Call `StorageClient.store(Storage storageAddress)` to
invoke a public function of another contract. The remote contract `UintStorage` saves the integer
value of the argument and the caller address in its state variables.

### 3. [Borrower](https://github.com/tonlabs/samples/blob/master/solidity/3_Borrower.sol): EVER transfer

This sample demonstrates how currency transfer works. Call `Borrower.askForALoan(Loaner loanerAddress, uint amount)`.
This requests `amount` of currency from the contract deployed at the specified address.
The remote contract [LoanerContract](https://github.com/tonlabs/samples/blob/master/solidity/3_Loaner.sol)
transfers `amount` of currency to the caller via `msg.sender.transfer(amount)`.
Each contract has an internal transaction counter. The counter value increases with each transaction
and is stored in the persistent memory.

### 4. [CurrencyExchange](https://github.com/tonlabs/samples/blob/master/solidity/4_CurrencyExchange.sol): callback implementation

Call `CurrencyExchange.updateExchangeRate(address bankAddress, uint16 code)`. This function allows
interacting with a remote contract by calling its function: `ICentralBank.GetExchangeRate(uint16 code)`.
The remote contract [CentralBank](https://github.com/tonlabs/samples/blob/master/solidity/4_CentralBank.sol)
obtains caller's address via `msg.sender` and performs a callback.

5. [Bank](https://github.com/tonlabs/samples/blob/master/solidity/5_Bank.sol): loan interaction between Bank and [BankClient](https://github.com/tonlabs/samples/blob/master/solidity/5_BankClient.sol)

Call `Bank.setAllowance(address bankClientAddress, uint amount)`.
Bank stores information about loan allowances and current debts for different contracts. This data
is recorded in the following state variable: `mapping(address => CreditInfo) clientDB;`  
A contract owner is supposed to call the `setAllowance()` function to specify limits.

[BankClient](https://github.com/tonlabs/samples/blob/master/solidity/5_BankClient.sol) is a client
that can interact with Bank.

Call `BankClient.getMyCredit(IBank bank)`.
This function calls the remote contract Bank to receive allowed credit limit via Bank invoking the
callback function `setCreditLimit(uint limit)`.

Call `BankClient.askForALoan(IBank bank, uint amount)`.
This function call the remote contract Bank to get an amount of credit. According to the current
credit info of the BankClient contract Bank will approve the credit via calling the callback
function "receiveLoan(uint n_totalDebt)" or refuse the credit via calling the callback function
`refusalCallback(uint availableLimit)`.
**receiveLoan** function also obtains balance of the contract via **address(this).balance** and
balance of the inbound message via **msg.value** and saves them in state variables.
**refusalCallback** function saves the argument (available credit limit) in the state variable.


### 6. [DataBase](https://github.com/tonlabs/samples/blob/master/solidity/6_DataBase.sol): exchange of different types of values

One of contract functions call allows sending to the [DataBaseClient](https://github.com/tonlabs/samples/blob/master/solidity/6_DataBaseClient.sol)
different values:
- uint64 array;
- five uint arrays;
- five uint256;
- struct array.

### 7. [Giver](https://github.com/tonlabs/samples/blob/master/solidity/7_Giver.sol): simple giver contract

This sample shows usage of different types of currency transactions and usage of a fallback function.

Call `Giver.transferToAddress(address payable destination, uint amount)` or
`Giver.do_tvm_transfer(address payable remote_addr, uint128 ton_value, bool bounce, uint16 sendrawmsg_flag)`
to perform a currency transaction.
Call `Giver.transferToCrashContract(address payable destination, uint amount)` to implement a crash
during transaction. That will cause an exception in [CrashContract](https://github.com/tonlabs/samples/blob/master/solidity/7_CrashContract.sol)
and Giver's contract fallback function calling.
Call `Giver.transferToAbstractContract(address payable destination, uint amount)` with a
non-existent address AbstractContract will also call a fallback function of Giver.

### 8. [Kamikaze](https://github.com/tonlabs/samples/blob/master/solidity/8_Kamikaze.sol): selfdestruct function

Call `Kamikaze.sendAllMoney(address anotherContract)`. This function destroys the contract and sends
all its funds to the specified address of [Heir](https://github.com/tonlabs/samples/blob/master/solidity/8_Heir.sol)
contract.

### 9. [PiggyBank](https://github.com/tonlabs/samples/blob/master/solidity/9_PiggyBank.sol): Piggy bank with two clients

This sample consists of 3 contracts:
- [PiggyBank](https://github.com/tonlabs/samples/blob/master/solidity/9_PiggyBank.sol) - piggy bank itself.
- [PiggyBank_Owner](https://github.com/tonlabs/samples/blob/master/solidity/9_PiggyBank_Owner.sol) - piggy bank's owner - valid user, who can add to piggy bank's deposit and withdraw.
- [PiggyBank_Stranger](https://github.com/tonlabs/samples/blob/master/solidity/9_PiggyBank_Stranger.sol) - stranger - invalid user, who can add to piggy bank but can not withdraw.

Call `PiggyBank_Owner.addToDeposit(PiggyBank bankAddress, uint amount)` or
`PiggyBank_Stranger.addToDeposit(PiggyBank bankAddress, uint amount)` to transfer EVERs from the
contract to PiggyBank.

Call `PiggyBank_Owner.withdrawDeposit(PiggyBank bankAddress)` of `PiggyBank_Stranger.withdrawDeposit(PiggyBank bankAddress)`
to try to withdraw the deposit from PiggyBank. Transfer would occur only for the owner.

### 10. [Wallet](https://github.com/tonlabs/samples/blob/master/solidity/10_Wallet.sol): Simple wallet

Call `Wallet.sendTransaction(address payable dest, uint128 value, bool bounce)`. This function
allows transferring EVERs to the specified account.

### 11. [ContractDeployer](https://github.com/tonlabs/samples/blob/master/solidity/11_ContractDeployer.sol): Deploy Contract from contract via `new`.

The way to get arguments for deploying is described [How to deploy contract from contract](https://github.com/tonlabs/samples/blob/master/solidity/17_ContractProducer.md).

### 12. [BadContract](https://github.com/tonlabs/samples/blob/master/solidity/12_BadContract.sol): Contract upgrade

Contract code could be changed via using **tvm.setcode** function. It could be useful for fixing
errors and functionality updating. In that example we have a [BadContract](https://github.com/tonlabs/samples/blob/master/solidity/12_BadContract.sol) (it is a [PiggyBank](https://github.com/tonlabs/samples/blob/master/solidity/9_PiggyBank.sol) contract with added upgrade functionality) and new version of that contract [NewVersion](https://github.com/tonlabs/samples/blob/master/solidity/12_NewVersion.sol).

Call "PiggyBank.setCode(TvmCell memory newcode)" with argument that contains code of [NewVersion](https://github.com/tonlabs/samples/blob/master/solidity/12_NewVersion.sol) contract to change the code of the contract.

### 13. [BankCollector](https://github.com/tonlabs/samples/blob/master/solidity/13_BankCollector.sol): Mapping methods

Developer can work with mappings using methods: **fetch**, **min**, **next**. This methods allow to
check existence of the key, obtain lexicographically minimal key and lexicographically next key
respectively.

### 14. [CustomReplayProtection](https://github.com/tonlabs/samples/blob/master/solidity/14_CustomReplayProtection.sol): Custom replay protection

Developer can redefine function **afterSignatureCheck** to create his own replay protection function
instead of default one.

### 15. [MessageSender](https://github.com/tonlabs/samples/blob/master/solidity/15_MessageSender.sol): Message construction and parsing

Developer can use TVM specific types to build message manually and special api function
**tvm.sendrawmsg()** to send it. Contract [MessageSender](https://github.com/tonlabs/samples/blob/master/solidity/15_MessageSender.sol) performs such actions to build a message which will call the function of another contract [MessageReceiver](https://github.com/tonlabs/samples/blob/master/solidity/15_MessageReceiver.sol). [MessageReceiver](https://github.com/tonlabs/samples/blob/master/solidity/15_MessageReceiver.sol)
also shows how to parse a cell.

### 16. [onBounceHandler](https://github.com/tonlabs/samples/blob/master/solidity/16_onBounceHandler.sol): Working with bounced messages

Developer can define **onBounce** function to work with bounced messages. If an error occurs while
message transferring or handling it can be bounced back to the source contract. This sample
demonstrates how you can handle such bounced message.

### 17. [low_level](https://github.com/tonlabs/samples/blob/master/solidity/17_low_level.md): Constructor message structure

### 18. [Interfaces](https://github.com/tonlabs/samples/blob/master/solidity/18_Interfaces.sol), [OrderClient](https://github.com/tonlabs/samples/blob/master/solidity/18_OrderClient.sol), [OrderDatabase](https://github.com/tonlabs/samples/blob/master/solidity/18_OrderDatabase.sol): Order Book

[**Interface.sol**](https://github.com/tonlabs/samples/blob/master/solidity/18_Interfaces.sol) - interface of [DataBase](https://github.com/tonlabs/samples/blob/master/solidity/18_OrderDatabase.sol) and [Client](https://github.com/tonlabs/samples/blob/master/solidity/18_OrderClient.sol) Contract.

[**OrderClient.sol**](https://github.com/tonlabs/samples/blob/master/solidity/18_OrderClient.sol) - contract that can create new orders in OrderDatabase contract.

[**OrderDtatabase.sol**](https://github.com/tonlabs/samples/blob/master/solidity/18_OrderDatabase.sol) - stores order information, creates an order, changes the database, etc.

### 19. [Casino](https://github.com/tonlabs/samples/blob/master/solidity/19_Casino.sol), [CasinoClient](https://github.com/tonlabs/samples/blob/master/solidity/19_CasinoClient.sol), [CasinoOwner](https://github.com/tonlabs/samples/blob/master/solidity/19_CasinoOwner.sol), [CasinoInterfaces](https://github.com/tonlabs/samples/blob/master/solidity/19_CasinoInterfaces.sol): Casino

[**Casino**](https://github.com/tonlabs/samples/blob/master/solidity/19_Casino.sol) - Casino roulette smart contract.

[**CasinoInterfaces.sol**](https://github.com/tonlabs/samples/blob/master/solidity/19_CasinoInterfaces.sol) - interface of the Casino contract.

[**CasinoOwner.sol**](https://github.com/tonlabs/samples/blob/master/solidity/19_CasinoOwner.sol), [**CasinoClient.sol**](https://github.com/tonlabs/samples/blob/master/solidity/19_CasinoClient.sol) - Casino owner smart contracts.

### 20. [Bomber](https://github.com/tonlabs/samples/blob/master/solidity/20_bomber.sol), [Interface](https://github.com/tonlabs/samples/blob/master/solidity/20_interface.sol), [Sink](https://github.com/tonlabs/samples/blob/master/solidity/20_sink.sol): Bomber



### 21. [Self_deploy](https://github.com/tonlabs/samples/blob/master/solidity/21_self_deploy.sol): Self Deployer

This sample shows how the contract can deploy another contract of the same type.

### 22. [Sender](https://github.com/tonlabs/samples/blob/master/solidity/22_sender.sol), [Sink](https://github.com/tonlabs/samples/blob/master/solidity/22_sink.sol): Sender

Generates cell which contains message which calls another contract by internal outbound message. 
Saves received value in state variable.

### 23. [rawReserve](https://github.com/tonlabs/samples/blob/master/solidity/23_rawReserve.sol), [sender](https://github.com/tonlabs/samples/blob/master/solidity/23_sender.sol): Reserver

A contract that reserves a certain amount of EVER from the remaining balance of the account: exactly,at most,or all but.

### 24. [ISquareProvider](https://github.com/tonlabs/samples/blob/master/solidity/24_ISquareProvider.sol), [LengthProvider](https://github.com/tonlabs/samples/blob/master/solidity/24_LengthProvider.sol), [SquareProvider](https://github.com/tonlabs/samples/blob/master/solidity/24_SquareProvider.sol), [WidthProvider](https://github.com/tonlabs/samples/blob/master/solidity/24_WidthProvider.sol): Rectangle square

### 25. [Config](https://github.com/tonlabs/samples/blob/master/solidity/25_Config.sol): Function arguments specification

Sometimes it can be not obvious in which way function arguments should be specified, especially if it is a large structure with different and complex fields. It is generally described in [abi doc](../../arch/abi/2.1.md). And this example was made to help users clear this moment.