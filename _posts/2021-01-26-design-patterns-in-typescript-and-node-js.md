---
layout: post
title: "TypeScript 및 Node.js의 디자인 패턴
 "
author: 'Code Tower'
thumbnail: https://blog.logrocket.com/wp-content/uploads/2021/01/typescript-node.png
tags: undefined
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/typescript-node.png?fit=750%2C487&ssl=1)

디자인 패턴은 소프트웨어 애플리케이션 개발에서 반복되는 문제에 대한 솔루션입니다.
 

우리 모두 알다시피 디자인 패턴에는 세 가지 유형이 있습니다.
 그들은:
 

- 창조
 
- 구조적
 
- 행동
 

하지만 기다려.
 그게 무슨 뜻이야?
 

창조 패턴은 우리가 객체 지향 스타일로 객체를 만드는 방법과 관련이 있습니다.
 클래스를 인스턴스화하는 방식으로 패턴을 적용합니다.
 

구조적 패턴은 클래스와 객체가 애플리케이션에서 더 큰 구조를 형성하도록 구성되는 방식과 관련이 있습니다.
 

행동 패턴은 객체가 단단히 결합되지 않고 어떻게 효율적으로 상호 작용할 수 있는지에 관한 것입니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/creational-structural-behavioral.png?resize=730%2C238&ssl=1)

이 튜토리얼은 Node.js 애플리케이션에서 사용할 수있는 가장 일반적인 디자인 패턴 중 일부를 설명합니다.
 구현을 더 쉽게하기 위해 Typescript를 사용할 것입니다.
 

## 하나씩 일어나는 것
 

싱글 톤 패턴은 클래스에 대해 하나의 인스턴스 만 있어야 함을 의미합니다.
 평신도 입장에서는 한 국가에 한 번에 한 명의 대통령 만 있어야합니다.
 이 패턴을 따르면 특정 클래스에 대한 여러 인스턴스를 피할 수 있습니다.
 

Singleton 패턴의 좋은 예는 애플리케이션의 데이터베이스 연결입니다.
 애플리케이션에 데이터베이스 인스턴스가 여러 개 있으면 애플리케이션이 불안정 해집니다.
 따라서 싱글 톤 패턴은 애플리케이션 전체에서 단일 인스턴스를 관리하여이 문제에 대한 솔루션을 제공합니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/singleton-shared-resources-stored-state.png?resize=730%2C342&ssl=1)

Typescript를 사용하여 Node.js에서 위의 예를 구현하는 방법을 살펴 보겠습니다.
 

```js
import {MongoClient,Db} from 'mongodb'
class DBInstance {

    private static instance: Db

    private constructor(){}

    static getInstance() {
        if(!this.instance){
            const URL = "mongodb://localhost:27017"
            const dbName = "sample"    
            MongoClient.connect(URL,(err,client) => {
                if (err) console.log("DB Error",err)
                const db = client.db(dbName);
                this.instance = db
            })

        }
        return this.instance
    }
}

export default DBInstance
```

여기에 속성 인스턴스가있는 클래스`DBInstance`가 있습니다.
 `DBInstance`에는 메인 로직이있는 정적 메소드`getInstance`가 있습니다.
 

이미 데이터베이스 인스턴스가 있는지 확인합니다.
 있는 경우 반환합니다.
 그렇지 않으면 데이터베이스 인스턴스를 만들고 반환합니다.
 

다음은 API 경로 내에서`singleton` 패턴을 사용하는 방법의 예입니다.
 

```js
import express,{  Application, Request, Response } from 'express'
import DBInstance from './helper/DB'
import bodyParser from 'body-parser'
const app = express()

async function start(){

    try{

        app.use(bodyParser.json())
        app.use(bodyParser.urlencoded({extended : true}))
        const db = await DBInstance.getInstance()

        app.get('/todos',async (req : Request,res : Response) => {
            try {
                const db = await DBInstance.getInstance()

                const todos = await db.collection('todo').find({}).toArray()
                res.status(200).json({success : true,data : todos})
            }
            catch(e){
                console.log("Error on fetching",e)
                res.status(500).json({ success : false,data : null })
            }
            
        })

        app.post('/todo',async (req : Request,res : Response) => {
            try {
                const db = await DBInstance.getInstance()

                const todo = req.body.todos
               const todoCollection =  await db.collection('todo').insertOne({ name : todo })

                res.status(200).json({ success : true,data : todoCollection })
            }
            catch(e){
                console.log("Error on Inserting",e)
                res.status(500).json({ success : false,data : null })
            }
        })

        app.listen(4000,() => {
            console.log("Server is running on PORT 4000")
        })
    }
    catch(e){
        console.log("Error while starting the server",e)
    }
}

start()
```

## 초록 공장
 

추상적 인 팩토리에 대한 설명을 시작하기 전에 `팩토리 패턴`이 의미하는 바를 알고 싶습니다.
 

### 간단한 공장
 

더 간단하게하기 위해 비유를하겠습니다.
 배가 고파서 음식을 원한다고 가정 해 봅시다.
 직접 요리하거나 식당에서 주문할 수 있습니다.
 이렇게하면 음식을 먹기 위해 요리하는 법을 배우거나 알 필요가 없습니다.
 

마찬가지로 Factory 패턴은 인스턴스화 논리를 클라이언트에 노출하지 않고 사용자에 대한 개체 인스턴스를 생성합니다.
 

이제 간단한 팩토리 패턴에 대해 알았으니 추상 팩토리 패턴으로 돌아가 보겠습니다.
 간단한 공장 예를 확장하여 배가 고파서 식당에서 음식을 주문하기로 결정했다고 가정 해 보겠습니다.
 취향에 따라 다른 요리를 주문할 수 있습니다.
 그런 다음 요리에 따라 최고의 레스토랑을 선택해야 할 수도 있습니다.
 

보시다시피 음식과 식당 사이에는 의존성이 있습니다.
 다른 레스토랑은 다른 요리에 더 좋습니다.
 

Node.js 애플리케이션 내에 `추상 팩토리`패턴을 구현해 보겠습니다.
 이제 우리는 다양한 종류의 컴퓨터가있는 노트북 가게를 만들 것입니다.
 몇 가지 주요 구성 요소는 `스토리지`와 `프로세서`입니다.
 

이를위한 인터페이스를 구축해 보겠습니다.
 

```undefined
export default interface IStorage {
     getStorageType(): string
}
import IStorage from './IStorage'
export default interface IProcessor {
    attachStorage(storage : IStorage) : string

    showSpecs() : string
}
```

다음으로 클래스에서`storage` 및`processor` 인터페이스를 구현해 보겠습니다.
 

```js
import IProcessor from '../../Interface/IProcessor'
import IStorage from '../../Interface/IStorage'

export default class MacbookProcessor implements IProcessor {

    storage: string | undefined

    MacbookProcessor() {
        console.log("Macbook is built using apple silicon chips")    
    }

    attachStorage(storageAttached: IStorage) {
        this.storage = storageAttached.getStorageType()
        console.log("storageAttached",storageAttached.getStorageType())
        return this.storage+" Attached to Macbook"
    }
    showSpecs(): string {
        return this.toString()
    }

    toString() : string {
        return "AppleProcessor is created using Apple Silicon and "+this.storage;
    }

}
import IProcessor from '../../Interface/IProcessor'
import IStorage from '../../Interface/IStorage'

export default class MacbookStorage implements IStorage {

    storageSize: number

    constructor(storageSize : number) {
        this.storageSize = storageSize
        console.log(this.storageSize+" GB SSD is used")
    }

    getStorageType() {
        return  this.storageSize+"GB SSD"
    }

}
```

이제`createProcessor` 및`createStorage`와 같은 메소드가있는 팩토리 인터페이스를 생성합니다.
 

```coffeescript
import IStorage from '../Interface/IStorage'
import IProcessor from '../Interface/IProcessor'

export default interface LaptopFactory {
    createProcessor() : IProcessor

    createStorage() : IStorage
}
```

팩토리 인터페이스가 생성되면 랩톱 클래스에서 구현하십시오.
 여기서는 다음과 같습니다.
 

```js
import LaptopFactory from '../../factory/LaptopFactory'
import MacbookProcessor from './MacbookProcessor'
import MacbookStorage from './MacbookStorage'

export class Macbook implements LaptopFactory {
    storageSize: number;

    constructor(storage : number) {
        this.storageSize = storage
    }

    createProcessor() : any{
        return new MacbookProcessor()
    }

    createStorage(): any {
        return new MacbookStorage(this.storageSize)
    }
}
```

마지막으로 팩토리 메서드를 호출하는 함수를 만듭니다.
 

```js
import LaptopFactory from '../factory/LaptopFactory'
import IProcessor from '../Interface/IProcessor'

export const buildLaptop =  (laptopFactory : LaptopFactory) : IProcessor => {
    const processor = laptopFactory.createProcessor()

    const storage = laptopFactory.createStorage()

    processor.attachStorage(storage)

    return processor
}
```

## 빌더 패턴
 

빌더 패턴을 사용하면 클래스에서 생성자를 사용하지 않고도 다양한 유형의 객체를 만들 수 있습니다.
 

하지만 왜 우리는 생성자를 사용할 수 없습니까?
 

글쎄, 특정 시나리오에서 `생성자`에 문제가 있습니다.
 `사용자`모델이 있고 다음과 같은 속성이 있다고 가정 해 보겠습니다.
 

```cpp
export default class User {

    firstName: string
    lastName : string
    gender: string
    age: number
    address: string
    country: string
    isAdmin: boolean

    constructor(firstName,lastName,address,gender,age,country,isAdmin) {
        this.firstName = builder.firstName
        this.lastName = builder.lastName
        this.address = builder.address
        this.gender = builder.gender
        this.age = builder.age
        this.country = builder.country
        this.isAdmin = builder.isAdmin
    }

}
```

이를 사용하려면 다음과 같이 인스턴스화해야 할 수 있습니다.
 

```cpp
const user = new User("","","","",22,"",false)
```

여기서 우리는 제한된 논쟁을 가지고 있습니다.
 그러나 일단 속성이 증가하면 유지하기가 어려울 것입니다.
 이 문제를 해결하려면 빌더 패턴이 필요합니다.
 

다음과 같은 빌더 클래스를 만듭니다.
 

```undefined
import User from './User'

export default class UserBuilder {

    firstName = ""
    lastName = ""
    gender = ""
    age = 0
    address = ""
    country = ""
    isAdmin = false

    constructor(){
        
    }

    setFirstName(firstName: string){
        this.firstName = firstName
    }

    setLastName(lastName : string){
        this.lastName = lastName
    }

    setGender(gender : string){
        this.gender = gender
    }

    setAge(age : number){
        this.age = age
    }

    setAddress(address : string){
        this.address = address
    }

    setCountry(country : string){
        this.country = country
    }

    setAdmin(isAdmin: boolean){
        this.isAdmin = isAdmin
    }

    build() : User {
        return new User(this)
    }

    getAllValues(){
        return this
    }
}
```

여기서는 `getter`와 `setter`를 사용하여 빌더 클래스의 속성을 관리합니다.
 그런 다음 모델 내에서 빌더 클래스를 사용하십시오.
 

```undefined
import UserBuilder from './UserBuilder'

export default class User {

    firstName: string
    lastName : string
    gender: string
    age: number
    address: string
    country: string
    isAdmin: boolean

    constructor(builder : UserBuilder) {
        this.firstName = builder.firstName
        this.lastName = builder.lastName
        this.address = builder.address
        this.gender = builder.gender
        this.age = builder.age
        this.country = builder.country
        this.isAdmin = builder.isAdmin
    }

}
```

## 어댑터
 

어댑터 패턴의 전형적인 예는 다른 모양의 전원 소켓입니다.
 때로는 소켓과 장치 플러그가 맞지 않습니다.
 작동하는지 확인하기 위해 어댑터를 사용합니다.
 이것이 바로 어댑터 패턴에서 수행 할 작업입니다.
 

다른 클래스와 호환되도록 어댑터에서 호환되지 않는 개체를 래핑하는 프로세스입니다.
 

지금까지 어댑터 패턴을 이해하는 비유를 보았습니다.
 어댑터 패턴이 생명의 은인이 될 수있는 실제 사용 사례를 보여 드리겠습니다.
 

`CustomerError` 클래스가 있다고 가정합니다.
 

```js
import IError from '../interface/IError'
export default class CustomError implements IError{

    message : string

    constructor(message : string){
        this.message = message
    }

    serialize() {
        return this.message
    }
}
```

이제 애플리케이션 전체에서이`CustomError` 클래스를 사용하고 있습니다.
 시간이 지나면 어떤 이유로 인해 수업에서 메소드를 변경해야합니다.
 

`New Custom Error` 클래스는 다음과 같습니다.
 

```cpp
export default class NewCustomError{

    message : string
    
    constructor(message : string){
        this.message = message    
    }

    withInfo() {
        return { message : this.message } 
    }
}
```

우리의 새로운 변경은 메서드를 변경하기 때문에 전체 응용 프로그램을 충돌시킵니다.
 이 문제를 해결하기 위해 어댑터 패턴이 작동합니다.
 

어댑터 클래스를 만들고이 문제를 해결해 보겠습니다.
 

```js
import NewCustomError from './NewCustomError'
// import CustomError from './CustomError'
export default class ErrorAdapter {
    message : string;
    constructor(message : string) {
        this.message = message
    }

    serialize() {
              // In future replace this function
        const e = new NewCustomError(this.message).withInfo()
        return e
    }

}
```

`serialize` 메소드는 전체 애플리케이션에서 사용하는 것입니다.
 우리의 응용 프로그램은 우리가 사용하는 클래스를 알 필요가 없습니다.
 `Adapter` 클래스가이를 처리합니다.
 

## 관찰자
 

관찰자 패턴은 다른 개체에 상태가 변경 될 때 종속 항목을 업데이트하는 방법입니다.
 일반적으로 `Observer`및 `Observable`이 포함됩니다.
 `Observer`는 `Observable`을 구독하고 변경 사항이있는 경우 Observable은 관찰자에게 알립니다.
 

![image](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/observable.png?resize=730%2C355&ssl=1)

이 개념을 이해하기 위해 관찰자 패턴의 실제 사용 사례를 살펴 보겠습니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/author.png?resize=730%2C415&ssl=1)

여기에는`Author`,`Tweet`,`follower` 엔티티가 있습니다.
 `팔로어`는 `저자`를 구독 할 수 있습니다.
 새로운 `트윗`이있을 때마다 `팔로어`가 업데이트됩니다.
 

Node.js 애플리케이션에서 구현해 보겠습니다.
 

```undefined
import Tweet from "../module/Tweet";

export default interface IObserver {
    onTweet(tweet : Tweet): string
}
import Tweet from "../module/Tweet";

export default interface IObservable{

    sendTweet(tweet : Tweet): any
}
```

여기에는`IObservable` 및`IObserver` 인터페이스가 있으며, 여기에는`onTweet` 및`sendTweet` 메소드가 있습니다.
 

```js
import IObservable from "../interface/IObservable";
import Tweet from "./Tweet";
import Follower from './Follower'
export default class Author implements IObservable {

    protected observers : Follower[] = []

    notify(tweet : Tweet){
        this.observers.forEach(observer => {
            observer.onTweet(tweet)
        })
    }

    subscribe(observer : Follower){
        this.observers.push(observer)
    }

    sendTweet(tweet : Tweet) {
        this.notify(tweet)
    }
}
```

`팔로어 .ts`
 

```js
import IObserver from '../interface/IObserver'
import Author from './Author'
import Tweet from './Tweet'

export default class Follower implements IObserver {

    name : string

    constructor(name: string){
        this.name = name
    }

    onTweet(tweet: Tweet) {
        console.log( this.name+" you got tweet =>"+tweet.getMessage())
        return this.name+" you got tweet =>"+tweet.getMessage()
    }

}
```

그리고`Tweet.ts` :
 

```cpp
export default class Tweet {

    message : string
    author: string

    constructor(message : string,author: string) {
        this.message = message
        this.author= author
    }

    getMessage() : string {
        return this.message+" Tweet from Author: "+this.author
    }
}
```

`index.ts`
 

```js
import express,{  Application, Request, Response } from 'express'
// import DBInstance from './helper/DB'
import bodyParser from 'body-parser'
import Follower from './module/Follower'
import Author from './module/Author'
import Tweet from './module/Tweet'

const app = express()

async function start(){

    try{

        app.use(bodyParser.json())
        app.use(bodyParser.urlencoded({extended : true}))
        // const db = await DBInstance.getInstance()

        app.post('/activate',async (req : Request,res : Response) => {
            try {

                const follower1 = new Follower("Ganesh")
                const follower2 = new Follower("Doe")

                const author = new Author()

                author.subscribe(follower1)
                author.subscribe(follower2)

                author.sendTweet(
                   new Tweet("Welcome","Bruce Lee")
                )

                res.status(200).json({ success : true,data:null })

            }
            catch(e){
                console.log(e)
                res.status(500).json({ success : false,data : null })
            }
        })

        app.listen(4000,() => {
            console.log("Server is running on PORT 4000")
        })
    }
    catch(e){
        console.log("Error while starting the server",e)
    }
}

start()
```

## 전략 패턴
 

전략 패턴을 사용하면 런타임에 알고리즘 또는 전략을 선택할 수 있습니다.
 이 시나리오의 실제 사용 사례는 파일 크기에 따라 파일 스토리지 전략을 전환하는 것입니다.
 

애플리케이션의 파일 크기에 따라 파일 스토리지를 처리하려고한다고 가정하십시오.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/uploading-file-size.png?resize=730%2C597&ssl=1)

여기서는 파일을 업로드하고 런타임 조건 인 파일 크기에 따라 전략을 결정하려고합니다.
 `전략`패턴을 사용하여이 개념을 구현해 보겠습니다.
 

`Writer` 클래스로 구현해야하는 인터페이스를 만듭니다.
 

```undefined
export default interface IFileWriter {
    write(filepath: string | undefined) : boolean
}
```

그 후 크기가 더 큰 경우 파일을 처리 할 `클래스`를 만듭니다.
 

```coffeescript
import IFileWriter from '../interface/IFileWriter'

export default class AWSWriterWrapper implements IFileWriter {

    write() {
        console.log("Writing File to AWS S3")
        return true
    }
}
```

그런 다음 크기가 더 작은 파일을 처리 할 `클래스`를 만듭니다.
 

```coffeescript
import IFileWriter from '../interface/IFileWriter'

export default class DiskWriter implements IFileWriter {

    write(filepath : string) {
        console.log("Writing File to Disk",filepath)
        return true
    }
}
```

두 가지가 모두 있으면 `전략`을 사용할 수있는 클라이언트를 만들어야합니다.
 

```js
import IFileWriter from '../interface/IFileWriter'

export default class Writer {

    protected writer
    constructor(writer: IFileWriter) {
        this.writer = writer
    }

    write(filepath : string) : boolean {
        return this.writer.write(filepath)
    }
}
```

마지막으로 다음과 같은 조건에 따라 전략을 사용할 수 있습니다.
 

```undefined
let size = 1000

                if(size < 1000){
                    const writer = new Writer(new DiskFileWriter())
                    writer.write("file path comes here")
                }
                else{
                    const writer = new Writer(new AWSFileWriter())
                    writer.write("writing the file to the cloud")
                }
```

### 책임의 사슬
 

책임 체인은 객체가 일련의 조건 또는 기능을 통과 할 수 있도록합니다.
 모든 기능과 조건을 한 곳에서 관리하는 대신 개체가 통과해야하는 조건 체인으로 분할됩니다.
 

이 패턴의 가장 좋은 예 중 하나는 `익스프레스 미들웨어`입니다.
 

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2021/01/express-middleware.png?resize=730%2C359&ssl=1)

우리는 익스프레스의 각 요청과 연결된 미들웨어로서의 기능을 구축합니다.
 우리의 요청은 미들웨어 내부의 조건을 통과해야합니다.
 `책임 사슬`의 가장 좋은 예입니다.
 

### 정면
 

파사드 패턴을 사용하면 단일 인터페이스 내에서 유사한 기능이나 모듈을 래핑 할 수 있습니다.
 이렇게하면 클라이언트가 내부적으로 어떻게 작동하는지 알 필요가 없습니다.
 

이에 대한 좋은 예는 컴퓨터를 부팅하는 것입니다.
 컴퓨터를 켰을 때 컴퓨터 내부에서 일어나는 일을 알 필요가 없습니다.
 버튼 하나만 누르면됩니다.
 그런 식으로 파사드 패턴은 클라이언트가 모든 것을 구현할 필요없이 높은 수준의 로직을 수행하는 데 도움이됩니다.
 

다음과 같은 기능이있는 사용자 프로필을 예로 들어 보겠습니다.
 

- 사용자 계정이 비활성화 될 때마다 상태를 업데이트하고 은행 세부 정보를 업데이트해야합니다.
 

`facade`패턴을 사용하여 애플리케이션에서이 로직을 구현해 보겠습니다.
 

```undefined
import IUser from '../Interfaces/IUser'

export default class User {
    private firstName: string
    private lastName: string
    private bankDetails: string | null
    private age: number
    private role: string
    private isActive: boolean

    constructor({firstName,lastName,bankDetails,age,role,isActive} : IUser){
        this.firstName = firstName
        this.lastName = lastName
        this.bankDetails = bankDetails
        this.age = age
        this.role = role
        this.isActive = isActive
    }

    getBasicInfo() {
        return {
            firstName: this.firstName,
            lastName: this.lastName,
            age : this.age,
            role: this.role
        }
    }

    activateUser() {
        this.isActive = true
    }

    updateBankDetails(bankInfo: string | null) {
        this.bankDetails= bankInfo
    }

    getBankDetails(){
        return this.bankDetails
    }

    deactivateUser() {
        this.isActive = false
    }

    getAllDetails() {
        return this
    }
}
export default interface IUser {
    firstName: string
    lastName: string
    bankDetails: string
    age: number
    role: string
    isActive: boolean
}
```

마지막으로 `facade`클래스는 다음과 같습니다.
 

```js
import User from '../module/User'

export default class UserFacade{

    protected user: User
    constructor(user : User){
        this.user = user
    }

    activateUserAccount(bankInfo : string){
        this.user.activateUser()
        this.user.updateBankDetails(bankInfo)

        return this.user.getAllDetails()
    }

    deactivateUserAccount(){
        this.user.deactivateUser()
        this.user.updateBankDetails(null)
    }

}
```

여기에서는 사용자 계정이 비활성화 될 때마다 호출해야하는 메서드를 결합합니다.
 

전체 소스 코드는 여기에서 찾을 수 있습니다.
 

## 결론
 

우리는 애플리케이션 개발에 일반적으로 사용되는 디자인 패턴 만 보았습니다.
 소프트웨어 개발에서 사용할 수있는 다른 디자인 패턴이 많이 있습니다.
 좋아하는 디자인 패턴과 사용 사례에 대해 자유롭게 의견을 말하십시오.
 