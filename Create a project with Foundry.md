# Reference
https://book.getfoundry.sh/
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
## 4.Create a file inside the ./src directory called Stake.sol. Paste the code below into the file
```shell
// SPDX-License-Identifier: MIT---------------------------
pragma solidity ^0.8.17;

import "@openzeppelin/token/ERC20/IERC20.sol";

error StakeFailed();

contract StakePool {
    //amount=deposits[user][token]
    mapping(address => mapping(address => uint256)) deposits;

    function stake(
        address token,
        uint256 amount
    ) external returns (bool success) {
        //unsafe
        success = IERC20(token).transferFrom(msg.sender, address(this), amount);
        if (!success) revert StakeFailed();
        deposits[msg.sender][token] += amount;
    }

    function unstake(address token) external {
        uint256 amount = deposits[msg.sender][token];
        deposits[msg.sender][token] = 0;
        IERC20(token).transfer(msg.sender, amount);
    }
}
```
![image](https://github.com/user-attachments/assets/01be20ae-73bb-4232-996b-074c684d193f)
## 5.Create a file inside the ./test directory called Stake.sol. Paste the code below into the file
```shell
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import "forge-std/Test.sol";
import "../src/Stake.sol";
import "./mocks/MockERC20.sol";

contract StakeTest is Test {
    StakePool stake;
    MockERC20 token;
    function setUp() public {
        stake = new StakePool();
        token = new MockERC20(1e8 * 1e18, "token", 18, "T");
    }

    function testStake(uint256 amount) public {
        vm.assume(amount < 1e8 * 1e18);
        //  uint256 amount= 1e18;

        token.approve(address(stake), amount);
        bool success = stake.stake(address(token), amount);
        assertTrue(success, "expect stake sucess");
    }
}
```
![image](https://github.com/user-attachments/assets/91d22b72-85eb-4ab5-b9f1-6348e73d97b6)
## 6.Create a mock folder in the .test directory and a MockERC20.sol file in the mock folder
```shell
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

interface Token {
    /// @param _owner The address from which the balance will be retrieved
    /// @return balance the balance
    function balanceOf(address _owner) external view returns (uint256 balance);

    /// @notice send `_value` token to `_to` from `msg.sender`
    /// @param _to The address of the recipient
    /// @param _value The amount of token to be transferred
    /// @return success Whether the transfer was successful or not
    function transfer(
        address _to,
        uint256 _value
    ) external returns (bool success);

    /// @notice send `_value` token to `_to` from `_from` on the condition it is approved by `_from`
    /// @param _from The address of the sender
    /// @param _to The address of the recipient
    /// @param _value The amount of token to be transferred
    /// @return success Whether the transfer was successful or not
    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) external returns (bool success);

    /// @notice `msg.sender` approves `_addr` to spend `_value` tokens
    /// @param _spender The address of the account able to transfer the tokens
    /// @param _value The amount of wei to be approved for transfer
    /// @return success Whether the approval was successful or not
    function approve(
        address _spender,
        uint256 _value
    ) external returns (bool success);

    /// @param _owner The address of the account owning tokens
    /// @param _spender The address of the account able to transfer the tokens
    /// @return remaining Amount of remaining tokens allowed to spent
    function allowance(
        address _owner,
        address _spender
    ) external view returns (uint256 remaining);

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(
        address indexed _owner,
        address indexed _spender,
        uint256 _value
    );
}

contract MockERC20 is Token {
    uint256 private constant MAX_UINT256 = 2 ** 256 - 1;
    mapping(address => uint256) public balances;
    mapping(address => mapping(address => uint256)) public allowed;
    uint256 public totalSupply;
    /*
    NOTE:
    The following variables are OPTIONAL vanities. One does not have to include them.
    They allow one to customise the token contract & in no way influences the core functionality.
    Some wallets/interfaces might not even bother to look at this information.
    */
    string public name; //fancy name: eg Simon Bucks
    uint8 public decimals; //How many decimals to show.
    string public symbol; //An identifier: eg SBX

    constructor(
        uint256 _initialAmount,
        string memory _tokenName,
        uint8 _decimalUnits,
        string memory _tokenSymbol
    ) {
        balances[msg.sender] = _initialAmount; // Give the creator all initial tokens
        totalSupply = _initialAmount; // Update total supply
        name = _tokenName; // Set the name for display purposes
        decimals = _decimalUnits; // Amount of decimals for display purposes
        symbol = _tokenSymbol; // Set the symbol for display purposes
    }

    function transfer(
        address _to,
        uint256 _value
    ) public override returns (bool success) {
        require(
            balances[msg.sender] >= _value,
            "token balance is lower than the value requested"
        );
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        emit Transfer(msg.sender, _to, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }

    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) public override returns (bool success) {
        uint256 allowance = allowed[_from][msg.sender];
        require(
            balances[_from] >= _value && allowance >= _value,
            "token balance or allowance is lower than amount requested"
        );
        balances[_to] += _value;
        balances[_from] -= _value;
        if (allowance < MAX_UINT256) {
            allowed[_from][msg.sender] -= _value;
        }
        emit Transfer(_from, _to, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }

    function balanceOf(
        address _owner
    ) public view override returns (uint256 balance) {
        return balances[_owner];
    }

    function approve(
        address _spender,
        uint256 _value
    ) public override returns (bool success) {
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }

    function allowance(
        address _owner,
        address _spender
    ) public view override returns (uint256 remaining) {
        return allowed[_owner][_spender];
    }
}
```
![image](https://github.com/user-attachments/assets/f345faf5-c88b-4a30-8916-780476c289b8)
## 7.Dependencies
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
![image](https://github.com/user-attachments/assets/9b4dd6e2-09c0-4f6f-a70b-808e36181c21)
### 5.Add the following code to the source file.(This code has been added to the source code.)
```shell
import "@openzeppelin/token/ERC20/IERC20.sol";
```
## 8.Build
```shell
forge build
```

## 9.Test
### 1.Test does not display detailed information
```shell
forge test
```
![image](https://github.com/user-attachments/assets/4c87376a-6089-49e3-9c38-c7c508664037)
### 2.Test and list detailed information
```shell
forge test -vvvv
```

