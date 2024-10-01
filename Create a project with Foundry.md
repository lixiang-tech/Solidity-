# Creating a New Project
## Update to the latest version
```shell
foundryup
```
## Initialize a project named hello_foundry 
```shell
forge init hello_foundry
cd hello_foundry
```
## Open VScode
```shell
code .
```
## Test
```shell
forge test
```
### Test and list detailed information
```shell
forge test -vvvv
```
## Build
```shell
forge build
```
## Dependencies
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
