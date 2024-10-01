# Creating a New Project
## 1.Update to the latest version
```shell
foundryup
```
## 2.Initialize a project named hello_foundry 
```shell
forge init hello_foundry
cd hello_foundry
```
## 3.Open VScode
```shell
code .
```
## 4.Test
```shell
forge test
```
## 5.Build
```shell
forge build
```
## 6.Dependencies
### 1.Install dependencies
```shell
forge install transmissions11/solmate Openzeppelin/openzeppelin-contracts --no-commit
```
### 2.View dependency paths
```shell
forge remappings
```
### 3.Write the dependency path to a remappings.txt file
```shell
forge remappings > remappings.txt
```
### 4.Replace the dependency path @openzeppelin/contracts/=lib/openzeppelin-contracts/contracts/
```shell
@openzeppelin/=lib/openzeppelin-contracts/contracts/
```
### 5.Add the following code to the source file
```shell
import "@openzeppelin/token/ERC20/IERC20.sol";
```
