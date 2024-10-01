# Reference
https://hardhat.org/tutorial
# catalogue
- <a href="#Setting up the environment">Setting up the environment</a>
- <a href="#Creating a new Hardhat project">Creating a new Hardhat project</a>
- <a href="#Writing and compiling smart contracts">Writing and compiling smart contracts</a>
- <a href="#Testing contracts">Testing contracts</a>
- <a href="#Deploying to localhost network">Deploying to localhost network</a>
- <a href="#Deploying to remote networks">Deploying to remote networks</a>

# <span id = "Setting up the environment">Setting up the environment</span> 
## 1.Installing WSL2（Windows Subsystem for Linux） on Windows11 system.
Run the Windows command window as an administrator and enter the command：
```shell
wsl --install 
``` 
Enter your name and password as prompted, and restart the system after installation is complete.

Open the Microsoft Store, search for Ubuntu, download Ubuntu 24.04 LTS, click the open button when the download is complete, enter your name and password as 
prompted, and restart the system after installation is complete.

Display the list of installed systems
```shell
wsl -l -v 
``` 
Remove the default installation of Ubuntu system and keep the latest system Ubuntu-24.04
```shell
wsl --unregister  Ubuntu  
``` 
Set the default version of the WSI system to 2
 ```shell
wsl --set-default-version 2 
```
Update and upgrade software packages  
 ```shell
sudo apt update && sudo apt upgrade 
```
Open the Microsoft Store, search for Windows Terminal and install it.

![image](https://github.com/user-attachments/assets/69683358-383f-4828-98d6-8858ee16e66c)

## 2.Configure the WSL system network and mirror it with the Windows system network
Create a Notepad in the C: \ User \ Username folder with the following content:
```shell
[wsl2]
networkingMode=mirrored
```
Save Notepad and change the file name to **.wslconfig**

Close the WSI system and reopen it after 8 seconds
```shell
wsl --shutdown
```
## 3.Install zsh/oh-my-zsh
Combined with oh-my-zsh and related plugins, zsh can achieve command highlighting, command completion, Git shortcut operations, and more.
Update package
```shell
sudo apt update && sudo apt upgrade
```
Install zsh
```shell
sudo apt install zsh -y
```
Install oh-my-zsh
```shell
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true
```
Install command completion and highlighting plugins
```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions
sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/g' ~/.zshrc
```
Set zsh as the default shell
```shell
chsh -s /bin/zsh 
```
Edit Profile **.zshrc**
```shell
vim .zshrc
```
Press the **i** key to start editing and adding content 
```shell
#Display all file details in a list format
alias ll="ls -alF"
#Confirm before deleting files
alias rm="rm -i"
```
After editing, press the **ESC** key and then press **:wq** to exit editing
## 4.Configure Git
Git ignores case by default, so configuration needs to be modified. 

#Enable case sensitivity
```shell
git config --global core.ignorecase false
```
General configuration

#Configure username and password
```shell
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
```
After saving the username and email, there is no need to enter them every time
```shell
git config --global credential.helper store
```
View configuration information
```shell
git config --global --list
```
## 5.Install nvm/node/npm
```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
```
```shell
nvm install 20
```
```shell
node -v
```
```shell
npm -v
```
## 6.VSCode
Install remote development extension package: 

https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack

Node.js extension package:

https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack

Use **code .** Command to open VSCode, note that the window displays WSL: Ubuntu-24.04 in the bottom left corner
```shell
code .
```
## 7.WSL file system
It should be noted that we now have two systems with different file types, and accessing and transferring files across systems will greatly reduce efficiency. It is best to store each system separately. Taking the user directory as an example:

If developing on Windows, place the files in C: \ Users \<UserName>\ 

If developing on Ubuntu, place the files in: \ \ wsl $\ ubuntu \ home \<UserName>\

# <span id = "Creating a new Hardhat project">Creating a new Hardhat project</span> 
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
## 6.installing Hardhat Plugins
```shell
npm install --save-dev @nomicfoundation/hardhat-toolbox
```
## 7.Open the VScode
```shell
code .
```
# <span id = "Writing and compiling smart contracts">Writing and compiling smart contracts</span> 
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
![image](https://github.com/user-attachments/assets/2ca57031-e113-40f0-8859-56d276f608bd)
## 2.Compiling contracts
```shell
npx hardhat compile
```
![image](https://github.com/user-attachments/assets/563c2790-b75c-4019-9b77-38e097839f24)

# <span id = "Testing contracts">Testing contracts</span> 
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
![image](https://github.com/user-attachments/assets/274d9602-a085-478d-aa9b-d1642351ddea)
## 2.In your terminal run npx hardhat test. You should see the following output
```shell
npx hardhat test
```
![image](https://github.com/user-attachments/assets/96a411dc-97c3-4d44-9c0e-8f9d1035830b)

# <span id = "Deploying to localhost network">Deploying to localhost network</span> 
## 1.Create a file inside the ./ignition/modules directory called Token.js. Paste the code below into the file
```shell
const { buildModule } = require("@nomicfoundation/hardhat-ignition/modules");

const TokenModule = buildModule("TokenModule", (m) => {
  const token = m.contract("Token");

  return { token };
});

module.exports = TokenModule;
```
![image](https://github.com/user-attachments/assets/8573cf60-5927-4cb9-8de5-f403582f10e2)

## 2.Replace the code below into the file hardhat.config.js
```shell
require("@nomicfoundation/hardhat-toolbox"); 
/** @type import('hardhat/config').HardhatUserConfig */ 
module.exports = { 
solidity: "0.8.24", 
networks: { 
hardhat: {}, 
}, 
};
```
![image](https://github.com/user-attachments/assets/80447744-f130-4ec1-b790-ac3ad72df972)
## 3.Start the local network
```shell
npx hardhat node
```
## 4.Open a new terminal and enter the following command to deploy to the local network
```shell
npx hardhat ignition deploy ./ignition/modules/Token.js --network localhost
```
![image](https://github.com/user-attachments/assets/6dec1d88-9d92-4961-8997-68cab1b32247)
# <span id = "Deploying to remote networks">Deploying to remote networks</span>.We’ll use Sepolia for this example. 
## 1.To use environment variables to save sensitive information in Hardhat, install dotenv.
```shell
npm install dotenv --save
```
## 2.Open VS Code, create a file named .env in the root directory of the project, and add the INFURA_ID and PRIVATE_KEY. INFURA_ID is obtained by registering an account on the INFURA official website. The PRIVATE_KEY is obtained from the MetaMask account.
```shell
INFURA_ID=***   
PRIVATE_KEY=**
```
![image](https://github.com/user-attachments/assets/d7978966-c393-4964-9ff6-77c844050ca9)
## 3.Replace the code below into the file hardhat.config.js
```shell
require("@nomicfoundation/hardhat-toolbox"); 
require("dotenv").config(); 
/** @type import('hardhat/config').HardhatUserConfig */ 
module.exports = { 
solidity: "0.8.24", 
networks: { 
hardhat: {}, 
sepolia: { 
url:
 "https://sepolia.infura.io/v3/" + process.env.INFURA_ID, accounts: [`0x${process.env.PRIVATE_KEY}`], 
 }, 
 }, 
 };
```
![image](https://github.com/user-attachments/assets/c2ccf80e-4045-4520-a794-7275e53aa6f3)
## 4.Deploy to test network Sepolia
```shell
npx hardhat ignition deploy ./ignition/modules/Token.js --network sepolia
```













