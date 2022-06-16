
# Hardhat notes/ cheatsheet

### Install 

`mkdir my-wave-portal
cd my-wave-portal
npm init -y
npm install --save-dev hardhat`


### Run HARDHAT

`npx hardhat`

this will generate some accounts for us on the node.


### use hardhat in you project 

import hardhat to the top of your sol file. 

`import "hardhat/console.sol";`


###  Test a smart contract steps

creates a test.js under scripts. 

1.  const fooContractFactory = await hre.ethers.getContractFactory("*- THIS WILL BE CONTRACT NAME IN SOL file  -*") this will complie and make an artifacts file. helps us work with contract 
2.  const fooContract = await fooContractFactory.deploy(); creates a local ether network. NOTE: everytime you run its a fresh blockchain. 
3. `constructor`  in the .sol will run 
4.  `fooContract.address` you can get the address of who deployed the contract. 



### Hardhat and hre

[Hardhat Hre docs](https://hardhat.org/advanced/hardhat-runtime-environment?utm_source=buildspace.so&utm_medium=buildspace_project)

- when writing a test script we use hre and ethers we do not need to import these as they are built on the fly by `hardhat.config.js`
  




### Keep local node ETH node running
- start new terminal in root hardhat project folder 
- `npx hardhat node`
- will create a local eth node with 10 addresses and private keys with 10000 ETH on 
- 