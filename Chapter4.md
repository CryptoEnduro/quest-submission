# CHAPTER 4 DAY 1
## THESE ARE MY ANSWERS FOR CHAPTER 4 DAY 1

#### 1. Explain what lives inside of an account.
- Inside the user account on the Flow blockchain live following things: 
    - digital assets (e.g. currencies or NFTs)
    - user data
    - smart contracts

#### 2. What is the difference between the /storage/, /public/, and /private/ paths?

- ```/storage/``` path can only by accessed by the account owner. All of user's data lives there.
- ```/public/``` path is available to everybody. The coder should handle the public path with special care not to let somebody access sensitive data.
- ```/private/``` - is the path available only to the account owner and the people that the account owner gives access to

#### 3. What does .save() do? What does .load() do? What does .borrow() do?
- ```.save``` saves data to the storage. It should use the actual piece od data and a /storage path as parameters
- ```.load``` takes data out from the storage and loads onto a variable. The coder needs to specify the type of data and the storage path. .load returns optional type. It is rarely used in Cadence development.
- ```borrow``` returns a reference to the date in the storage. It is a very common practice to work with data using ```.borrow``` method. The coder needs to specify the type of data with the & (ref sign) and the storage path

#### 4. Explain why we couldn't save something to our account storage inside of a script.
A script cannot modify the state. It can only view the state. It can return some value. It is free and does not cost gas. Saving to storage can be accomplish inside the contract or in transactions.

#### 5. Explain why I couldn't save something to your account.
My account is private and the /storage space where I can save my data is private. By default all data and objects are stored in private storage. The owner has a possibility to open up access to /storage to others using capabilities that access the public contract methods and data. Since I didn't use capabilities, you can't save anything to my account.

#### 6. Define a contract that returns a resource that has at least 1 field in it.
```
pub contract MyStorage {

  pub resource Test {
    pub var name: String
    pub var count: UInt

    init() {
      self.name = "Test3"
      self.count = 0
    }
  }

  pub fun createTest(): @Test {
    return <- create Test()
  }

}
```
#### Then, write 2 transactions:
- A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.
```
import MyStorage from 0x01

transaction() {

  prepare(signer: AuthAccount) {

    let testResource <- MyStorage.createTest()
    signer.save(<- testResource, to: /storage/MyTestResource) 
    // saves `testResource` to my account storage at this path:
    // /storage/MyTestResource

    let testResource2 <- signer.load<@MyStorage.Test>(from: /storage/MyTestResource)
                     ?? panic("A `@MyStorage.Test` resource does not live here.")
    // takes `testResource` out of my account storage

    // log the 'name' field of the resource
    log(testResource2.name)

    // destroy resource
    destroy testResource2

  }

  execute {

  }
}

```

- A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.
import MyStorage from 0x01


```
transaction() {

  // I added some extra logic to the code for a test purpose.
  prepare(signer: AuthAccount) {

    // get the reference of the resource. It is possible that there is no resource in the storage. Let's see...
    var testResource1: &MyStorage.Test? = signer.borrow<&MyStorage.Test>(from: /storage/MyTestResource)
    
    // if there is no resource of type MyStorage.Test saved at '/storage/MyTestResource'
    if testResource1 == nil {

      // create resource
      let testResource <- MyStorage.createTest()

      // get the reference of the newly created resource
      signer.save(<- testResource, to: /storage/MyTestResource) 
      let testResource2 = signer.borrow<&MyStorage.Test>(from: /storage/MyTestResource)
                          ?? panic("A `@MyStorage.Test` resource does not live here.")

      // log the field 'count' from the resource
      log(testResource2.count)
      log("The resource didn't exist before. We have just created it")

    } else {     // if there was already a resource at '/storage/MyTestResource', log the field 'name' from the resource

        log(testResource1!.name)  
        // unwraps optional and shows name
    }
  }

  execute {

  }
}
```

# CHAPTER 4 DAY 2
## THESE ARE MY ANSWERS FOR CHAPTER 4 DAY 2

#### 1. What does .link() do?
.link() allows us to make a resource in /storage publicly available to other users (accounts). It is important to remember that other accounts cannot move or destroy the object! They can only access fields that the owner has explicitly declared in the type specification of the link method.
When we .link() the resource to the /public/ path, we can restrict access to certain fields and methods of the resource by using a resource interface.

#### 2. In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.
While using the .link() method we can specify resoure interface after the resource reference by placing it between the brackets {}. The resource interface can restrict access to some of the variables and functions of the resource that we make publicly available. It can also restrict access to the whole resource.

#### 3. Deploy a contract that contains a resource that implements a resource interface. 
```
access(all) contract MyStorage {

  pub resource interface ITest {
    pub var name: String
    access(all) fun changeName (name: String)
  }
  
  pub resource Test: ITest {
    pub var name: String
    pub var count: UInt

    access(all) fun incrementCount () {
      self.count = self.count + 1
    } 

    access(all) fun changeName (name: String) {
      self.name = name
    }

    init() {
      self.name = ""
      self.count = 0
    }
  }

  pub fun createTest(): @Test {
    return <- create Test()
  }

}
```
#### Then, do the following:
    1. In a transaction, save the resource to storage and link it to the public with the restrictive interface.
```
import MyStorage from 0x01

transaction() {

  prepare(acct: AuthAccount) {

    let testResource: @MyStorage.Test <- MyStorage.createTest()
    
    testResource.changeName(name: "cat")
    testResource.incrementCount()
    
    acct.save(<- testResource, to: /storage/MyTestResource) 

    acct.link<&MyStorage.Test{MyStorage.ITest}>(/public/MyTestResource, target: /storage/MyTestResource)   

  }
  execute {
  }
}
```
    2. Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.

 ![Screenshot from 2022-07-21 21-23-03](https://user-images.githubusercontent.com/109131706/180299752-e326fc21-018e-4b35-9dc5-4542d5dc7e33.png)

    3. Run the script and access something you CAN read from. Return it from the script.
 
 ![Screenshot from 2022-07-21 21-32-04](https://user-images.githubusercontent.com/109131706/180301395-fe25568f-2944-4ddc-bb16-c86db14983ec.png)
 

# CHAPTER 4 DAY 3
## THESE ARE MY ANSWERS FOR CHAPTER 4 DAY 3

#### 1.Why did we add a Collection to this contract? List the two main reasons.
1. To be able to better organize our NFTs and not having to memorize a unique /storage path for each NFT that we minted
2. To be able to mint NFTs for other people and give or sell our NFTs to them, thus making NFTs into real digital assets. 

#### 2. What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")
You need to remember about declaring a 'destroy' function for the nested NFTs that manually destroys them with the 'destroy' keyword.

#### 3. Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.

    Idea #1: Do we really want everyone to be able to mint an NFT? thinking.
    No, not really unless it is for marketing or charity purposes. You might want however that a certain group od people can mint their NFTs for free, e.g. players in a play-to-earn game. 

    Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?
    No, I do not think so. It wold be probably good to declare a function inside the resource that returns some important information about the NFT, e.g. current owner, current price, linked digital document or piece of art etc.
    
    Idea #3. It would be good to add a function 'buy' that would be working together with the minting and transferring mechanism. This way we could sell NFTs to other users.
    
    Idea #4. It could be a good idea to borrow an NFT to some kind of a reseller. This reseller could take our NFT to a market place and sell it for profit and we would either receive a fixed price for the NFT or a participate in a profit sharing scheme.

    Idea #5. We could add a so called reverse auction mechanism to our NFT, where the price of the NFT would be automatically updated periodically becoming cheaper and cheaper after each period, in case the NFT didn't find a buyer. The process would continue until the minimal price is reached or a buyer bought our NFT :)
    
    Idea #6. Maybe the most important one. It would be useful to link the NFT to a digital art piece or a digital document, so that the NFT has actually some value on the marketplace.

 
  

