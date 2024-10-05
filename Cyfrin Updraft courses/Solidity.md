reference: https://updraft.cyfrin.io/courses/solidity/storage-factory/solidity-imports

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
