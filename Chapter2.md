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

- force unwrap operator causes ```Panic``` if the value is ```nil```, or assigns the value to a variable with a specific type (e.g. String) that should not be optional any more. The force-unwrap operator also returns the value inside of an optional, and if there is a nil value, it then panics and aborts the program.


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
