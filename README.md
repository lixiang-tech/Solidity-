# References
https://hardhat.org/tutorial
# Creating a new Hardhat project
## 1.Create a new folder
```shell
mkdir hardhat-tutorial
cd hardhat-tutorial
```
## 2.Initialize an npm project
```shell
npm init -y
```
## 3.Install Hardhat
```shell
npm i -D hardhat
```
## 4.Run Hardhat
```shell
npx hardhat init
```
## 5.Select Create a JavaScript project
![image](https://github.com/user-attachments/assets/f21173fa-4449-434f-b329-b5faba1efee5)
## 6.Open the VScode
```shell
code .
```
# Writing and compiling smart contracts
## 1.Create a file inside the ./contracts directory called Token.sol. Paste the code below into the file
```shell
//SPDX-License-Identifier: UNLICENSED

// Solidity files have to start with this pragma.
// It will be used by the Solidity compiler to validate its version.
pragma solidity ^0.8.0;


// This is the main building block for smart contracts.
contract Token {
    // Some string type variables to identify the token.
    string public name = "My Hardhat Token";
    string public symbol = "MHT";

    // The fixed amount of tokens, stored in an unsigned integer type variable.
    uint256 public totalSupply = 1000000;

    // An address type variable is used to store ethereum accounts.
    address public owner;

    // A mapping is a key/value map. Here we store each account's balance.
    mapping(address => uint256) balances;

    // The Transfer event helps off-chain applications understand
    // what happens within your contract.
    event Transfer(address indexed _from, address indexed _to, uint256 _value);

    /**
     * Contract initialization.
     */
    constructor() {
        // The totalSupply is assigned to the transaction sender, which is the
        // account that is deploying the contract.
        balances[msg.sender] = totalSupply;
        owner = msg.sender;
    }

    /**
     * A function to transfer tokens.
     *
     * The `external` modifier makes a function *only* callable from *outside*
     * the contract.
     */
    function transfer(address to, uint256 amount) external {
        // Check if the transaction sender has enough tokens.
        // If `require`'s first argument evaluates to `false`, the
        // transaction will revert.
        require(balances[msg.sender] >= amount, "Not enough tokens");

        // Transfer the amount.
        balances[msg.sender] -= amount;
        balances[to] += amount;

        // Notify off-chain applications of the transfer.
        emit Transfer(msg.sender, to, amount);
    }

    /**
     * Read only function to retrieve the token balance of a given account.
     *
     * The `view` modifier indicates that it doesn't modify the contract's
     * state, which allows us to call it without executing a transaction.
     */
    function balanceOf(address account) external view returns (uint256) {
        return balances[account];
    }
}
```
![image](https://github.com/user-attachments/assets/344173cf-3390-467c-af53-0194f505e674)
## 2.Compiling contracts
```shell
npx hardhat compile
```
![image](https://github.com/user-attachments/assets/e9d18e40-512e-4a9b-99cc-b8aca4e7cee3)
# Testing contracts
## 1.Create a file inside the ./test directory called Token.js. Paste the code below into the file
```shell
const { expect } = require("chai");

describe("Token contract", function () {
  it("Deployment should assign the total supply of tokens to the owner", async function () {
    const [owner] = await ethers.getSigners();

    const hardhatToken = await ethers.deployContract("Token");

    const ownerBalance = await hardhatToken.balanceOf(owner.address);
    expect(await hardhatToken.totalSupply()).to.equal(ownerBalance);
  });
});
```
![image](https://github.com/user-attachments/assets/3db34aa8-ac3c-41eb-bb50-50b181b757f9)
## 2.In your terminal run npx hardhat test. You should see the following output
![image](https://github.com/user-attachments/assets/5912e04f-9e59-4db8-9d12-b8a9b39ab70d)
# Deploying to localhost network
## 1.Create a file inside the ./ignition/modules directory called Token.js. Paste the code below into the file
```shell
const { buildModule } = require("@nomicfoundation/hardhat-ignition/modules");

const TokenModule = buildModule("TokenModule", (m) => {
  const token = m.contract("Token");

  return { token };
});

module.exports = TokenModule;
```
![image](https://github.com/user-attachments/assets/d66f2839-dfbf-456b-a907-9e1963ff9fb7)
## 2.Start the local network
```shell
npx hardhat node
```
## 3.Open a new terminal and enter the following command to deploy to the local network
```shell
npx hardhat ignition deploy ./ignition/modules/Token.js --network localhost
```
![image](https://github.com/user-attachments/assets/6dec1d88-9d92-4961-8997-68cab1b32247)









