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

# CHAPTER 2 DAY 3
## THESE ARE MY ANSWERS FOR CHAPTER 2 DAY 3

#### 1. In a script, initialize an array (that has length == 3) of your favourite people, represented as ```String```s, and ```log``` it.
```
pub fun main(){

    var s: [String] = ["Ola", "Jacob", "Piotr"]

    log(s)
}
```

#### 2. In a script, initialize a dictionary that maps the ```String```s Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a ```UInt64``` that represents the order in which you use them from most to least. For example, YouTube --> 1, Reddit --> 2, etc. If you've never used one before, map it to 0!

```
pub fun main(){

    var usage: {String: UInt64} = {"Facebook": 2, "Instagram": 3, "Twitter": 4, "You Tube": 1, "Reddit": 5, "LinkedIn": 6}
    
}
```

#### 3. Explain what the force unwrap operator ! does, with an example different from the one I showed you (you can just change the type).

- force unwrap operator causes ```Panic``` if the value is ```nil```, or assigns the value to a variable with a specific type (e.g. String) that should not be optional any more.


#### 4. Using this picture below, explain...
##### - What the error message means
        The error message means that there is a type mismatch because optional cannot be returned as String 
##### - Why we're getting this error
        Because we didn't use the unwrap operator (1st possibility)
        or we didn't declare the return value of the function as optional (2nd possibility)
##### - How to fix it</br>
        - Use the unwrap operator ! after thing[0x03]  (1st possibility)
        - declare the return value of the function as String? (2nd possibility)
        - or use ?? panic() (3rd possibility)
```
// 1st possibility
pub fun main(): String {

    let thing: {Address: String} = {0x01: "one", 0x02: "two", 0x03: "three"}
    return thing[0x03]!
}
```
or
```
// 2nd possibility
pub fun main(): String? {

    let thing: {Address: String} = {0x01: "one", 0x02: "two", 0x03: "three"}
    return thing[0x03]
}
```     
or
```
// 3rd possibility
pub fun main(): String {

    let thing: {Address: String} = {0x01: "one", 0x02: "two", 0x03: "three"}
    return thing[0x03] ?? panic("It looks like there is nothing to return")
}
```  



![Screenshot from Jacob](https://github.com/emerald-dao/beginner-cadence-course/blob/main/chapter2.0/images/wrongcode.png)


# CHAPTER 2 DAY 4
## THESE ARE MY ANSWERS FOR CHAPTER 2 DAY 4

1. Deploy a new contract that has a Struct of your choosing inside of it (must be different than Profile).
2. Create a dictionary or array that contains the Struct you defined.
3. Create a function to add to that array/dictionary.

```
pub contract Library {
   
    pub var index: UInt32
    pub var indexes: {UInt32: Book}

    pub struct Book {
        pub let author: String
        pub let title: String
        pub let nrPages: UInt16
        pub let yearPublished: UInt16

        init(_author: String, _title: String, _nrPages: UInt16, _yearPublished: UInt16) {
            self.author = _author
            self.title = _title
            self.nrPages = _nrPages
            self.yearPublished = _yearPublished
        }
    }

    pub fun incrementIndex() {        
        self.index = self.index + 1
    }

    pub fun addBook(author: String, title: String, nrPages: UInt16, yearPublished: UInt16) {
        let newBook = Book(_author: author, _title: title, _nrPages: nrPages, _yearPublished: yearPublished)
        self.indexes[self.index] = newBook
    }


    init() {
        self.index = 0
        self.indexes = {}
    }

}
```

4. Add a transaction to call that function in step 3.

```
import Library from 0x01

transaction(author: String, title: String, nrPages: UInt16, yearPublished: UInt16) {

    prepare(signer: AuthAccount) {}

    execute {
        Library.addBook(author: author, title: title, nrPages: nrPages, yearPublished: yearPublished)
        Library.incrementIndex()
        log("Added successfully")
    }
}
```


5. Add a script to read the Struct you defined.

```
import Library from 0x01

pub fun main() {
  
 for key in Library.indexes.keys {
    let value = Library.indexes[key]!
    log(key)
    log(value)
 }

}
```
# CHAPTER 3 DAY 1
## THESE ARE MY ANSWERS FOR CHAPTER 3 DAY 1

#### 1. In words, list 3 reasons why structs are different from resources.
1. Structs are less secure than resources and it is much easier to deal with them. For example, structs can be copied, resources cannot.
2. Structs can be lost, forgotten or overwritten. Resources cannot. You always need to keep track of resources.
3. Structs and resources use diffrent operators that have different meanings. Structs use ```=``` as an assignment operator, resources on the other hand, use ```<-``` to move them around. You also have to use a ```@``` symbol in front of a resource's type. 

#### 2. Describe a situation where a resource might be better to use than a struct.
It is better to use resources than structures when we deal with some unique and precious data, like for example NFTs. We want to make sure each NTF is unique, has a distinct ```uuid```  and cannot be copied. We do not want for example that someone fakes our Emerald Academy diploma. 

#### 3. What is the keyword to make a new resource?
The keyword is ```create```.

#### 4. Can a resource be created in a script or transaction (assuming there isn't a public function to create one)?
No, it cannot.

#### 5. What is the type of the resource below?
Hmm, I would say this is not a resource. This is only a declaration of a public resource. A resource must be explicitly created with the keyword ```create```.
```
pub resource Jacob {

}
```
#### 6. Let's play the "I Spy" game from when we were kids. I Spy 4 things wrong with this code. Please fix them.
Here is the fixed code with some comments
```
pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): @Jacob { // add @ in front of Jacob
        let myJacob <- create Jacob() // replace = with <-, add the keyword create
        return <- myJacob // add the move operator <-
    }
}
```

# CHAPTER 3 DAY 2
## THESE ARE MY ANSWERS FOR CHAPTER 3 DAY 2

#### 1. Write your own smart contract that contains two state variables: an array of resources, and a dictionary of resources. Add functions to remove and add to each of them. They must be different from the examples above.

```
pub contract Treasure {

    pub var arrayOfGems: @[Gem]
    pub var dictionaryOfGems: @{String: Gem}

    pub resource Gem {
        pub let gemName: String
        pub let gemOrigin: String
        pub let possibleToBuy: Bool
        pub let gemValue: UInt32

        init(_gemName: String, _gemOrigin: String, _possibleToBuy: Bool, _gemValue: UInt32) {
            self.gemName = _gemName
            self.gemOrigin = _gemOrigin
            self.possibleToBuy = _possibleToBuy
            self.gemValue = _gemValue
        }
    }

    pub fun createGem (gemName: String, gemOrigin: String, possibleToBuy: Bool, gemValue: UInt32): @Gem {
        var newGem: @Gem <- create Gem(_gemName: gemName, _gemOrigin: gemOrigin, _possibleToBuy: possibleToBuy, _gemValue: gemValue)
        return <- newGem
    }

    pub fun addGemToArr(gem: @Gem) {
        self.arrayOfGems.append(<- gem)
    }

    pub fun removeGemFromArr(index: Int): @Gem {
        return <- self.arrayOfGems.remove(at: index)
    }

    pub fun addGemToDict(gem: @Gem) {
        let key = gem.gemName
        let oldGem <- self.dictionaryOfGems[key] <- gem
        destroy oldGem
    }

    pub fun removeGemFromDict(key: String): @Gem {
        let gem <- self.dictionaryOfGems.remove(key: key) ?? panic("Could not find the gem!")
        return <- gem
    }

    init() {
        self.arrayOfGems <- []
        self.dictionaryOfGems <- {}
    }

}
```

# CHAPTER 3 DAY 3
## THESE ARE MY ANSWERS FOR CHAPTER 3 DAY 3

#### 1. Define your own contract that stores a dictionary of resources. Add a function to get a reference to one of the resources in the dictionary.
```
pub contract CryptoGems {

    pub var dictionaryOfGems: @{String: Gem}

    pub resource Gem {
        pub let name: String
        pub(set) var value: UInt32

        init(_name: String, _value: UInt32) {
            self.name = _name
            self.value = _value
        }
    }

    // name: String, value: UInt32
    pub fun createGem (name: String, value: UInt32): @Gem {
        var newGem: @Gem <- create Gem(_name: name, _value: value)
        return <- newGem
    }

    pub fun addGemToDict(name: String, newGem: @Gem) {
        let oldGem <- self.dictionaryOfGems[name] <- newGem
        destroy oldGem
    }

    pub fun changeValue(newValue: UInt32, gemRef: &Gem) {
        gemRef.value = newValue
    }

    pub fun getGemRef(name: String): &Gem? {
        let gemRef: &Gem? = &self.dictionaryOfGems[name] as &Gem?
        return gemRef
    }
  

    init() {
        self.dictionaryOfGems <- {}
    }
}
```

#### 2. Create a script that reads information from that resource using the reference from the function you defined in part 1.

```
import CryptoGems from 0x01

pub fun main (name: String, newValue: UInt32) {
 
    var gemRef: &CryptoGems.Gem? = CryptoGems.getGemRef(name: name)
    CryptoGems.changeValue(newValue: newValue, gemRef: gemRef!)
    log(gemRef)
  
}
```
The transaction below is not a part of the solution, but it might help testing the code
```
import CryptoGems from 0x01
transaction (name: String, value: UInt32) {
 
  prepare(signer: AuthAccount) {}

 
  execute {

    var gem: @CryptoGems.Gem <- CryptoGems.createGem(name: name, value: value)
    CryptoGems.addGemToDict(name: name, newGem: <- gem)
    log("Operation successful, ".concat(name).concat(" added"))
  
  }
}
```

#### 3. Explain, in your own words, why references can be useful in Cadence.
References in Cadence are useful for many reasons
- Not moving resources (and other data types) around, thus keeping them safe
- Accessing resources (and other data types) with ease and efficiently e.g., changing some properties of the resource without moving it back and forth
- Speed, it is usually faster to access data via a reference
- Saving gas

