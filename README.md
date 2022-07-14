# quest-submission

# CHAPTER 1 DAY 1
## THESE ARE MY ANSWERS FOR CHAPTER 1 DAY 1

### Explain what the Blockchain is in your own words.

Blockchain is a distributed database stored on the nodes of a computer network. It is composed of blocks that contain sets of data. Blocks are being created periodically and are linked with each other through cryptoghrapic method. A block contains its own hash (a cryptographic number created from the block's data) and the hash of the previous block. Blockchain is secure and immutable. It means that data once entered into the blockchain cannot be changed. Blockchain is usually used to store transaction. 
It is very difficult or even impossible to fake data in a blockchain, because of cryptography and because of the physical and geographical distribution of the blochain's nodes. Nodes of a blockchain communicate with each other and they use so callled consensus mechanism to ensure that there exist only one single valid copy of record shared by all nodes.
Blockchain is public. It means that anyone can view the data on it.

### Explain what a Smart Contract is.

Smart contract is a programs stored on a blockchain. Once written to the blockchain it cannot be modified. It allows anonymous parties to carry out transactions and agreements without the need for a third party (e.g. bank) or external enforcement mechanism. They run when predetermined conditions are met. They can automate workflow i.e. execute a task when conditions are met.

### Explain the difference between a script and a transaction.

A script views the state only. It cannot modify the state. It can return some value. It is free and does not cost gas. It usually has a public function main that returns a value.
A transaction can view and modify the state. It costs gas. It usually interacts with smart contract on the blochchain. Transaction in Cadence usually has two phases: prepare and execute.
In Cadence script, contracts and transactions are written separately.

# CHAPTER 1 DAY 2
## THESE ARE MY ANSWERS FOR CHAPTER 1 DAY 2

### What are the 5 Cadence Programming Language Pillars?

1. Safety and Security: Cadence language is very secure. It has strong type system, separation between contracts and transactions, and Resource Oriented Programming.
2. Clarity: Cadence code is easy to read and easy to verify that it's bug free and secure.
3. Approachability: Cadence syntax is similar to other programming lanugages making it easy to use by programmers.
4. Developer Experience: Un case of an error in the code, Candence gives very clear error messages that are easy to follow.
5. Resource Oriented Programming. It is probably the most important feature of Cadence.

### In your opinion, even without knowing anything about the Blockchain or coding, why could the 5 Pillars be useful?
Yes, absolutely. They sound like perfect guidelines for creating a beautiful code. And if I ever decide to write my own programming language, I will for sure take the pillars into consideration.

# CHAPTER 2 DAY 1
## THESE ARE MY ANSWERS FOR CHAPTER 2 DAY 1

### Deploy a contract to account 0x03 called "JacobTucker". Inside that contract, declare a constant variable named is, and make it have type String. Initialize it to "the best" when your contract gets deployed.


```
// Contract
pub contract JacobTucker {

    pub let is: String

    init() {
        self.is = "the best"
    }
}


// This transaction shows that I deployed the contract to account 0x03

import JacobTucker from 0x03

transaction() {

  prepare(signer: AuthAccount) {}

  execute {
    log("Jacob Tucker is ".concat(JacobTucker.is))
  }
}

```

### Check that your variable is actually equals "the best" by executing a script to read that variable. Include a screenshot of the output.

```
// Script
import JacobTucker from 0x03

pub fun main(): String {
    return JacobTucker.is
}
```

![Screenshot from 2022-07-14 22-19-25](https://user-images.githubusercontent.com/109131706/179078612-4aaffa80-876f-4660-8424-4ce312866948.png)

# CHAPTER 2 DAY 2
## THESE ARE MY ANSWERS FOR CHAPTER 2 DAY 2

#### Explain why we wouldn't call changeGreeting in a script.
changeGreetings function is changing the state variable ```greeting``` thus we cannot call it from a script, only from a transation. A script views the state only. It cannot modify the state. 

#### What does the ```AuthAccount``` mean in the ```prepare``` phase of the transaction?
It is the account/person that is signing the transaction and paying for it. When a user signs a transaction, the transaction takes his ```AuthAccount``` and can access the data in the user's account.

#### What is the difference between the ```prepare``` phase and the ```execute``` phase in the transaction?
The purpose of the ```prepare``` phase is to access the information/data in the user's account. The ```execute``` phase can't do that. It can call functions and do stuff to change the data on the blockchain. It is important to notice, that we actually don't need the ```execute``` phase. Technically, all can be done in the ```prepare``` phase. This way however, the code wouldn't be clear.

#### Add two new things inside your contract:
A variable named ```myNumber``` that has type ```Int``` (set it to 0 when the contract is deployed)
A function named ```updateMyNumber``` that takes in a new number named ```newNumber``` as a parameter that has type ```Int``` and updates ```myNumber``` to be ```newNumber```

```
// Contract
pub contract HelloWorld {

    pub var greeting: String
    pub var myNumber: Int

    pub fun changeGreeting(newGreeting: String) {
        self.greeting = newGreeting
    }

    pub fun updateMyNumber(newNumber: Int) {
        self.myNumber = newNumber
    }

    init() {
        self.greeting = "Hello, World!"
        self.myNumber = 0
    }
}

```
#### Add a script that reads ```myNumber``` from the contract
```
// New script
import HelloWorld from 0x01

pub fun main(): Int {
    return HelloWorld.myNumber
}
```

#### Add a transaction that takes in a parameter named ```myNewNumber``` and passes it into the ```updateMyNumber``` function. Verify that your number changed by running the script again.

```
// New transaction
import HelloWorld from 0x01

transaction(myNewNumber: Int) {

  prepare(signer: AuthAccount) {}

  execute {
    HelloWorld.updateMyNumber(newNumber: myNewNumber)
  }
}
```

![Screenshot from 2022-07-15 00-07-25](https://user-images.githubusercontent.com/109131706/179097062-449600d0-e574-491f-afdc-71f12f8103d0.png)
