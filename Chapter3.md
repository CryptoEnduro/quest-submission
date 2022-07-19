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
No, it cannot due to security reasons and also because creating a resource causes the modifcation of the state. A script cannot modify the state.
The only possibility to create a resource via a transaction or a script is through a public function in the contract. 

#### 5. What is the type of the resource below?
This is a resource of type @Jacob
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

# CHAPTER 3 DAY 4
## THESE ARE MY ANSWERS FOR CHAPTER 3 DAY 4

#### 1. Explain, in your own words, the 2 things resource interfaces can be used for (we went over both in today's content)

- A resource interfeces "sits" on top of a resource. It contains function and fields that a resource implementing the interface must provide implementations for. Thus it can be seen as the "resource specifications" or "requirements"
- Resource interface can restrict access to some of the variables and functions of the resource to certain people. It can also restrict access to the whole resource.

#### 2. Define your own contract. Make your own resource interface and a resource that implements the interface. Create 2 functions. In the 1st function, show an example of not restricting the type of the resource and accessing its content. In the 2nd function, show an example of restricting the type of the resource and NOT being able to access its content.

```
access(all) contract UFO {

    pub resource interface IAlien {
        pub let name: String
        pub let species: String

        access(contract) fun logTopSecret(): String
    }

    pub resource Alien: IAlien {
        pub let name: String
        pub let age:    UInt8
        pub let species: String
        pub let topSecret: String

        pub fun logTopSecret (): String {
            return (self.topSecret)
        }       

        init(_name: String, _age: UInt8, _species: String, _topSecret: String) {
            self.name = _name
            self.age = _age
            self.species = _species
            self.topSecret = _topSecret
        }
    }

    // 1st function. The type of the resource is not restricted here, and the function can access its content
    pub fun ShowRestrictedData (alien: @Alien) {
        log(alien.age)
        log(alien.topSecret)
        destroy alien
    }

    // 2nd function. The function below will give us an error, because we have restricted the type of the resource
    // and, at the same time, we are trying to access the restricted variable alien.age.
    // Although the alien.topSecret variable is also restricted, we are able to access its value by using a resource 
    // function that is accessible from inside the contract
    pub fun tryToshowRestrictedData (alien: @Alien{IAlien}) {
        log(alien.age)
        var topSecret = alien.logTopSecret()
        log("The top secret for ".concat(alien.name).concat(" is ").concat(topSecret))
        destroy alien
    }

    pub fun createAlien (name: String, age: UInt8, species: String, topSecret: String): @Alien {
        var newAlien: @Alien <- create Alien(_name: name, _age: age, _species: species, _topSecret: topSecret)
        return <- newAlien
    }

    init() {
        
    }
}
```

#### 3. How would we fix this code?

In order to fix the code below I implemented two changes: 
1. I commented out the variable ```favouriteFruit```, that was defined inside the interface but not implemented inside the sturcture. Since this variable is not used in the contract, the best I could do was to get rid of it.
2. I defined a function ```changeGreeting()``` inside the interface with the ```access(contract)``` modifier, making it possible to call the function from inside the contract.

```
pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      // pub var favouriteFruit: String
      access(contract) fun changeGreeting(newGreeting: String): String
    }

    // ERROR:
    // `structure Stuff.Test does not conform 
    // to structure interface Stuff.ITest`
    pub struct Test: ITest {
      pub var greeting: String

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting // returns the new greeting
      }

      init() {
        self.greeting = "Hello!"
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
      log(newGreeting)
    }
}
```
# CHAPTER 3 DAY 5
## THESE ARE MY ANSWERS FOR CHAPTER 3 DAY 5

#### For today's quest, you will be looking at a contract and a script. You will be looking at 4 variables (a, b, c, d) and 3 functions (publicFunc, contractFunc, privateFunc) defined in SomeContract. In each AREA (1, 2, 3, and 4), I want you to do the following: for each variable (a, b, c, and d), tell me in which areas they can be read (read scope) and which areas they can be modified (write scope). For each function (publicFunc, contractFunc, and privateFunc), simply tell me where they can be called.

```pub(set) var a: String```
This variable can be read, written and modified in Area 1, 2, 3 and 4

```pub var b: String```
This variable can be read in Area 1, 2, 3 and 4, and it can be modified in Area 1 only.

```access(contract) var c: String```
This variable can be read in Area 1, 2 and 3, and it can be modified in Area 1 only.

```access(self) var d: String```
This variable can be read and modified in Area 1 only.

```publicFunc``` 
This function is visible in Area 1,2,3 and 4.

```contractFunc``` 
This function is visible in Area 1,2 and 3.

```privateFunc```
This function is visible in Area 1 only

![Help diagram](https://github.com/emerald-dao/beginner-cadence-course/raw/main/chapter3.0/images/boxdiagram.PNG)
