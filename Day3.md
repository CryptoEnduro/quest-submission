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
