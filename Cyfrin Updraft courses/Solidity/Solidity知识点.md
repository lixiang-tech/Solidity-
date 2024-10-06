参考网址: https://updraft.cyfrin.io/courses/solidity/storage-factory/solidity-imports

SimpleStorage.sol :
```solidity
contract SimpleStorage {}

contract SimpleStorage2 {}

contract SimpleStorage3 {}

contract SimpleStorage4 {}
```

StorageFactory.sol :
```solidity
import {SimpleStorage,SimpleStorage2} from "./SimpleStorage.sol"
```
# 测试题
**1. What must be included when overriding a method from a parent contract in Solidity?** The function name, parameters, visibility, return type, and override keyword.

**2. How are the keywords override and virtual used together in Solidity?** override is used in a parent contract to mark functions that can be overridden, while virtual is used in a child contract to override those functions.

**3. What does the new keyword tell to the compiler?** It tells the compiler that a new contract instance is intended to be deployed after compilation.

# 关键词 override virtual
关键词override用在父合约函数中，关键词virtual用在子合约函数中
![图片](https://github.com/user-attachments/assets/d2a48905-a36b-40f4-ba57-4900a26c57eb



