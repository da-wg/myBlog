---
title: "cryptoZombies [solidity]"
date: 2021-02-06T11:40:29-05:00
draft: true
categories: ["coding"]
series: ["solidity"]
ShowToc: true
---
[CryptoZombies's Website](https://cryptozombies.io/)

---
**Note: I already have a little bit of programming experience so I will be relating some of the concepts to Javascript/C++ concepts. A great resource for learning Javascript is freeCodeCamp's Javascript Algorithms and Data Structures Certification course**

---
## Lesson 1
Solidity's code is encapsulated in contracts.  
**contract:** fundamental building block of Ethereum applications - all variables and functions belong to a contract. Starting point of all your projects  
**pragma solidity:** version of Solidity compiler you wish to use
**state variables** - permanently stored in contract storage  
**structs:** - objects

**arrarys:** - two types: 
- fixed arrays: `Person[2] fixedArrary;`
- dynmaic arrays: `Person[] people;`
- public arrays: Solidity automatically create a getter method: `Person[] public people;`
- .push() to add to back of array

**functions:** - things to note: 
- when passing in by reference, use `memory`
- start function parameter names with _
- good practice to mark private as default (name private function starting with _)
- `returns (string memory)`: return type 
- view: view-only, no modifications (const in C++)
- pure: not access any data in the app (Solidity is good at telling you when to use view vs. pure)

**keccak256:** built in hash function. One purpose is for pseudo-random number generation. Note: not secure.  
**typecasting:** converting between data types  
**events**: way for your contract to communicate that something happened on the blockchain to your app front-end, which can be 'listening' for certain events and take action when they happen. 
**Web3.js:** Javascript library for Ethereum that allows you to write a frontend that interacts with the contract. 

```solidity
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {
    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function _createZombie(string memory _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        emit NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string memory _str) private view returns (uint) {
        uint rand = uint(keccak256(abi.encodePacked(_str)));
        return rand % dnaModulus;
    }

    function createRandomZombie(string memory _name) public {
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}
```

---
## Lesson 2
**addresses:** Owned by a specific user, we can use it as a unique ID for ownership (set to the Ethereum address that called the function)  
**mapping:** Key-value store for storing and looking up date (dictionaries)  
**import:** same as import in Javascript  
**interface:** used to interact with another contract on the blockchain (syntactically the same as interfaces in C++). **Note:** In Solidity, you can return more than one value from a function

functions:
- **msg.sender:** address of the person (or smart contract) who called the current function. In Solidity, function execution always needs to start with an external caller so there will always be a msg.sender. Gives you security of the Ethereum blockchain (only way to modify somebody elses' data is to steal their private key)
- **require:** function stops executing if some condition is not true (like Racket error checking). **Note:** Solidity does not have native string comparison  
- **inheritance:** same as inheritance in C++  
- **storage vs. memory:** storage is permanent, memory is temporary (eg. hard drive vs RAM or local vs global variable)
- **internal and external:**
  - internal is same as private, but accessible to contracts that inherit this contract
  - external is like public, but functions can ONLY be called from outside the contract

zombiefactory.sol: 
```Solidity
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    mapping (uint => address) public zombieToOwner;
    mapping (address => uint) ownerZombieCount;

    function _createZombie(string memory _name, uint _dna) internal {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        zombieToOwner[id] = msg.sender;
        ownerZombieCount[msg.sender]++;
        emit NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string memory _str) private view returns (uint) {
        uint rand = uint(keccak256(abi.encodePacked(_str)));
        return rand % dnaModulus;
    }

    function createRandomZombie(string memory _name) public {
        require(ownerZombieCount[msg.sender] == 0);
        uint randDna = _generateRandomDna(_name);
        randDna = randDna - randDna % 100;
        _createZombie(_name, randDna);
    }

}
```

zombiefeeding.sol:
```Solidity
pragma solidity >=0.5.0 <0.6.0;

import "./zombiefactory.sol";

contract KittyInterface {
  function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
  );
}

contract ZombieFeeding is ZombieFactory {

  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  KittyInterface kittyContract = KittyInterface(ckAddress);

  function feedAndMultiply(uint _zombieId, uint _targetDna, string memory _species) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;

    if(keccak256(abi.encodePacked(_species)) == keccak256(abi.encodePacked("kitty"))) {
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
  }

  function feedOnKitty(uint _zombieId, uint _kittyId) public {
    uint kittyDna;
    (,,,,,,,,,kittyDna) = kittyContract.getKitty(_kittyId);

    feedAndMultiply(_zombieId, kittyDna, "kitty");
  }

}
```

---
## Lesson 3
After you deploy a contract, it's immutable, meaning it can never be modified or updated again. If there's a flaw in your contract code, there's no way for you to patch it later. You would have to tell your users to start using a different smart contract address that has the fix.
Because of this, it is often smart to have a function that allows up to change addresses that rely on external dependencies, in case something happens to those contracts in the future. 

**Ownable:** contracts that give owners special privileges: 
- modifiers: executes before the main function gets executed (mostly used to add require checks). 
- onlyOwner: only the owner can call the function
**OpenZeppelin:** library of secure and community-vetted smart contracts that you can use in your own DApps.

**Gas:**
- how much gas is required to execute a function depends on how complex the function's logic is, based on how much computing resources will be required to perform that operation (eg. writing to storage is much more expensive than adding two ints)
- total gas cost is the sum of the gas costs of all its individual operations
- code optimization is much more important in Ethereum than any other programming language because there is a real cost to users
- in structs, using smaller-sized uint will allow Solidity to pack these variables together to take up less storage. You also want to cluster identical data types together. 

**Time Units:**
- now: unix timestamp of latest block
- Solidity also has seconds, minutes, hours, days, weeks, years as default
- 

