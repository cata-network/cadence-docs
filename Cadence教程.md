## 1.First Steps

在这个教程中，我们会学到如何去使用智能合约，切换账户，并且学会浏览用户状态。



---

### **什么是Cadence？**



Cadence是一个全新的智能合约编程语言，专门为Flow区块链使用的语言。为了让开发者确保他们的代码是安全的，保密的，清晰的并且容易实现的。Cadence会让初次了解智能合约的开发者介绍几个新的特性:

+ 类型安全并且是强静态类型。
+ 面向资源编程，一种新的范例，它将线性系统与对象功能配对，通过确保资源（及其相关资产）一次只能存在于一个位置、不能被复制、不会意外丢失或删除，为数字所有权创建一个安全的声明性模型。
+ 在函数和[交易](https://docs.onflow.org/cadence/language/transactions/ )中，内置的前置条件和后置条件。
+ 基于能力的安全性应用，对象的访问权限仅限于所有者和那些有有效引用的人去施加访问控制。

请参阅[Cadence介绍](https://docs.onflow.org/cadence )来了解关于这门语言的更多设计理念。



### **什么是Flow开发者平台？**

[Flow开发者平台](https://play.onflow.org/)包括一个内置浏览器编辑器和一个体验Flow的模拟器。使用Flow开发者平台



### **了解开发者平台**

这个平台有任何你想知道的关于部署Cadence智能合约和如何与交易中的脚本交互的知识。

平台预装了合约和交易的模版，并且在文档页添加了对应的教程。为了从特定的教程了解智能合约，可以点击在平台网页的顶部的“Examples”链接了解更多。这个链接会打开一个包含所有教程的菜单。

当您单击这些链接之一时，教程将在新选项卡中打开里面包含的合约、交易和脚本将加载到 Playground 的模板中供您使用。

### **账户和合同**

账户模块在屏幕的左侧，在这里可以看到和选择激活的账户。在这个主菜单你也可以点进去进入账户选项卡去看该账户涉及到他其他账户。

![img](https://storage.googleapis.com/flow-resources/documentation-assets/cadence-tuts/playground-accounts.png)

当你有了Cadence代码后，在账户编辑模块中可以看到包含了一个合约，你也可以点击屏幕右下角的部署按钮去部署到一个已经选择的账户。

![img](https://storage.googleapis.com/flow-resources/documentation-assets/cadence-tuts/playground-deploy.png)

几秒过后，可以在终端看到一条确认合约部署的信息。你也可以看到合约名称显示在该账户的选项卡中，表明该账户现在已经部署了该合约。

您还可以从左侧选择菜单中选择交易和脚本并提交它们以与您部署的智能合约进行交互，这部分将在 Hello World 教程中介绍。

这只是您可以使用 开发者平台 执行的一小部分操作。如果您想了解不同 开发者平台 功能的更详细说明，请查看 开发者平台手册。

### **资源**

这个模块的每个教程都使用了包含交易，合约，脚本的文件。您在教程中使用我们提供的所有代码都可以复制粘贴或者你可以使用开发者平台预先生成的教程设置。

### **Say Hello, World!**

现在您可以运行Flow开发者平台，您可以为 Flow 创建一个智能合约！



## **2.Hello World**

---

### **让我们写一个并部署我们第一个智能合约**

>在Flow开发者平台打开本教程的入门代码：
>
>https://play.onflow.org/83315e10-8a9d-40d1-9f4a-877ca93eb93c
>
>本教程将要求您采取多种操作来与此代码进行交互。

>需要您采取行动的说明总是包含在这样的标注框中。
>
>这些突出显示的操作是让代码运行所需的全部操作，但阅读其余部分对于理解语言的设计是必要的。

智能合约是一段可验证和可执行并且无需可信任第三方的程序。在区块链上运行的程序通常被称为智能合约，因为它们可以调解重要的功能（例如货币）而无需依赖中央机构（例如银行）。

在开始在Flow上编程前，我们需要知道账户和交易是如何创建起来的。

### **账户和交易**

---

像许多其他区块链一样，Flow的设计模式也是以账户和交易为中心的。所有持久状态全都存储在账户中。接口（交互状态的方式）也存储在账户中。所有代码的运行都发生在交易中，交易是外部用户提交的代码块，用于与持久状态交互，包括直接修改帐户存储的状态。

每个账户都会被一个或多个私钥绑定。这意味着协议本身默认就支持了[多重签名](https://www.coindesk.com/what-is-a-multisignature-crypto-wallet)绑定账户/钱包。

一个账户分为两个主要的模块：

1. 第一个模块是[合同模块](https://docs.onflow.org/cadence/language/accounts/)。这个模块存放了所有的智能合约，包括合约的类型定义，作用域，还有与之相关具有相同功能的函数。这个模块也拥有合约的接口，这些接口基本上是其他合约导入和实现的程序指南。合同模块不能直接从交易中读取到，否则交易仅仅是读取到部署在账户上面的代码。账户的所有者可以直接添加、删除或更新/覆盖存储在其中的合约。
2. 第二个模块是账户的文件系统。这个模块是账户存储他们拥有的对象和控制如何访问这些对象功能的地方。

对象存放在[文件系统下的路径](https://docs.onflow.org/cadence/language/accounts/#paths)。路径由域名和标识符构成。

路径起始字符是 `/`，然后紧跟域名，路径分隔符用`/`，最后是标识符。举个栗子，路径`/storage/test`域名为`storage`标识符为`test`。

只有三个有效的域名分别代表了账户文件系统三个不同的模块：`storage`, `private` ,  `public`。

标识符是自定义的，可以是您想要使用的任何名称，以指示该路径中存储的内容。

+ `storage`域名是一个存储所有对象（例如代表着tokens和NFTs的结构体和资源对象）的路径。它仅能被账户所有者直接访问。
+ `private`域名就像一个私有API。您可以选择在此处将[功能定位符](https://docs.onflow.org/cadence/language/capability-based-access-control/)（capabilities）存储到您存储的任何资产中。仅有所有者或者给予授权的人才可以使用这个接口去调用定义在你的私人资产里的函数。
+ `public`域名就像一个账户公有API。所有者可以在此处链接功能定位符，网络中的任何其他人都可以访问这些功能定位符，以便与存储在帐户中的私有资产进行交互。

![img](https://storage.googleapis.com/flow-resources/documentation-assets/cadence-tuts/accounts-diagram.png)

在Flow中，一笔[交易(transaction)](https://docs.onflow.org/cadence/language/transactions/)被定义成一个具有任意大小，可以被一个或多个账户签名的代码块。交易可以访问那些已经签名过的账户的`/storage`和 `/private`域名，并且可以读写这些域名，同时还可以在读和调用这些公有合约的函数和其他用户公用域名。



### **创建一个智能合约**

---

我们从写一个返回`"Hello World!"`公有函数来创建我们的第一个智能合约。

>首先，您需要按照此链接打开一个带有预加载的 Hello World 合约、交易和脚本的平台会话：{" "}
>
>https://play.onflow.org/83315e10-8a9d-40d1-9f4a-877ca93eb93c

>打开账户 `0x01` 的标签页可以看到名为 `HelloWorld.cdc`的文件。
>
>`HelloWorld.cdc` 应该包含如下代码

`HelloWorld.cdc`

```Cadence
pub contract HelloWorld {

    // Declare a public field of type String.
    //
    // All fields must be initialized in the init() function.
    pub let greeting: String

    // The init() function is required if the contract contains any fields.
    init() {
        self.greeting = "Hello, World!"
    }

    // Public function that returns our friendly greeting!
    pub fun hello(): String {
        return self.greeting
    }
}
```

在Flow中，一个合约就是代码（函数）和数据（状态）的集合。合约的作用域只在一个账户下的合约模块中。账户可以有一个或者多个合约和合约接口，并且这些合约及合约接口可以由账户所有者免费地添加，修改和删除。

`pub let greeting: String `这行声明了一个公有（`pub`）状态常量（`let`）叫做`greeting`字符串类型。如果我们想要声明一个变量可以使用`var`。

`pub` 关键字是一个访问控制格式的例子，这意味着在它可以在所有作用域下访问，但是并不能在所有作用域下写入。如果你更喜欢带有描述性的关键字，你也可以使用`access(all)`来替换`pub`

请参阅[语言参考：访问控制部分](https://docs.onflow.org/cadence/language/access-control/)，了解有关 Cadence 中允许的不同级别的访问控制的更多信息。

`init()`这部分称为构造函数，这个函数只有当合约创建时运行并且之后不会在运行。这个 栗子中，构造函数设置了`greeting`值为`"Hello, World!"`。

下一个声明的公有函数返回了一个`String`类型的值。导入这个合约的任何人都可以访问这个公有域，使用公有类型，然后调用公有合约函数；这就是声明`pub`或者`access(all)` 的作用。

现在我们可以将此合约部署到您的帐户并在交易中调用其函数。



### **部署代码**

---

现在您已经有一些可以运行的Cadence代码，你可以部署它到您的账户上。

>确认账户页`0x01`存在并且已经编辑完成`HelloWorld.cdc`文件
>
>现在让我们点击部署按钮把编辑器里的内容部署到账户`0x01`上。

![img](https://storage.googleapis.com/flow-resources/documentation-assets/cadence-tuts/playground-deploy.png)

您应该会在输出区域中看到一条日志，表明部署成功。 （如果交易号或区块不同，请不要担心。）

```Cadence
11:18:34 Deployment > [1] > Deployed Contract To: 0x01
```

你也可以看到以该名字命名的合约已经出现在所选账户的下面。这表示`HelloWorld` 合约已经成功部署到该账户。你也可以点进这个账户标签页去确认哪些合约在哪些账户中，但每个账户只能有一个合约。



### **创建一个交易**

---

>打开名为`Say Hello`的交易
>
>`Say Hello`应该包含如下代码

`Say`

```Cadence
import HelloWorld from 0x01

transaction {

    // No need to do anything in prepare because we are not working with
    // account storage.
    prepare(acct: AuthAccount) {}

    // In execute, we simply call the hello function
    // of the HelloWorld contract and log the returned String.
    execute {
        log(HelloWorld.hello())
    }
}
```

这就是一个Cadence的**交易**。一个交易可以包含导入来自其他账户，与账户存储交互，与其他账户交互等的任何代码。

为了与智能合约交互，这个交易首先导入通过从存储它的地址检索其定义来导入该智能合约。这个导入包含接口的定义，资源定义，和公有函数，以便交易可以使用它们与合约本身或使用该合约的其他账户进行交互。

为了导入来自其他账户的智能合约，你可以写入这一行：

```Cadence
import {合约名称} from {账户地址}
```

交易可以分为两个阶段：`prepare`（准备阶段）和`execute`（执行阶段）。

1. `prepare`准备阶段是唯一可以访问签名帐户的私有 `AuthAccount`（授权账户） 对象的地方。`AuthAccount`（授权账户）有特殊的方法，允许从`/storage`读出和写入，创建`/private`和`/public`链接到`/storage`中的对象。（称之为[功能定位符](https://docs.onflow.org/cadence/language/capability-based-access-control)（capabilities），之后会介绍）
2. `execute`执行阶段无法访问`AuthAccount`，因此只能修改在准备阶段删除的对象并调用外部合约和对象上的函数。

通过在准备阶段不允许访问账户存储，我们可以静态地验证对于指定的交易哪些资产和认证的存储区域可以修改。对于用户而言，在浏览器钱包和应用程序中提交的交易可以使用这种方法来显示交易可能改变的内容，并且用户可以更有信心地相信他们不会通过应用程序生成的交易获得恶意攻击。

您也可以在平台上通过单击多个帐户头像来拥有多个交易签名者，但是需要交易的准备的代码块参数数量需要与签名者的数量相同。如果没有，这将导致错误。

在这个交易中，我们从它部署到的地址导入合约并调用它的 `hello` 函数。

>在编辑器右上角的框中，选择账户 `0x01` 作为交易签名者。 点击发送`Send`按钮提交交易

![img](https://storage.googleapis.com/flow-resources/documentation-assets/cadence-tuts/playground-signers.png)

您应该会看到如下内容：

```
"Hello, World!"
```

恭喜您，您刚才运行了你的第一个Cadence交易！:100:



### **创建一个资源**

---

接下来，我们将要练习使用[资源](https://docs.onflow.org/cadence/language/composite-types/#resources)，这是Cadence定义的新特性之一。一个资源包含多个基本类型就像一个结构体或者类，但还有些特别的规则。

>打开账户页`0x22`可以看到名为`HelloWorldResource.cdc`的文件
>
>`HelloWorldResource.cdc`应该包含如下的代码：

`HelloWorldResource.cdc`

```Cadence
pub contract HelloWorld {

  // Declare a resource that only includes one function.
  pub resource HelloAsset {
    // A transaction can call this function to get the "Hello, World!"
    // message from the resource.
    pub fun hello(): String {
      return "Hello, World!"
    }
  }

  init() {
    // Use the create built-in function to create a new instance
    // of the HelloAsset resource
    let newHello <- create HelloAsset()

    // We can do anything in the init function, including accessing
    // the storage of the account that this contract is deployed to.
    //
    // Here we are storing the newly created HelloAsset resource
    // in the private account storage
    // by specifying a custom path to the resource
    self.account.save(<-newHello, to: /storage/Hello)

    log("HelloAsset created and stored")
  }
}
```

>使用`Deploy按钮`把代码部署到账户`0x02`上

这是合约的另一个用处。Cadence 可以在部署的合约中声明类型定义。任何帐户都可以导入这些定义并使用它们与这些类型的对象进行交互。该合约声明了 `HelloAsset` 资源的定义。

让我们来看看这个合约：

```
pub resource HelloAsset {
    pub fun hello(): String {
        return "Hello, World!"
    }
}
```

资源是复合类型就像结构体和类一样，因为它们可以包含任意数量的字段或函数。唯一的区别就是代码如何与它们交互。当直接在所有者创建的时候它们都是很有用的。但事实上每一个资源的实例都只能存在一份并且不能被复制。在它们被访问的时候，必须显式地明确资源被修改或者移动，只有这样才能确保避免发生意外丢失。

其他传统编程语言的结构对于支持所有权可能不是一个理想的方式，因为他们可以被复制。这意味着一个编程上的错误会导致把一个相同的资产创建了很多的副本，这会打破了资产具有真正价值所需的稀缺性。我们必须要思考避免发生一个房子，一辆车或者一个具有百万美元的银行账户，甚至一匹马的损失和盗窃事件发生。因此，资源（Resource），这个数据结构，通过显式表达资产的创建，销毁和移动来解决这个问题。

```Cadence
init(){
// ...
```

之前的例子已经看到，声明了`init()`构造函数。所有的复合类型像合约（contracts），资源（resources）和结构体（structs），都可以有一个可选的 `init()` 函数，该函数仅在最初创建对象时运行。

合约还可以通过使用内置的 `self.account` 对象将它们部署到的帐户的存储进行读写访问。这是一个`AuthAccount`（授权账户）对象，可以使他们访问许多不同的功能，以与帐户的私有存储进行交互。

在合约的`init`构造函数中，合约使用`create`关键字去创建一个`HelloAsset`类型的实例并且将其保存到局部变量。为了创建一个新的资源对象，我们使用`create`关键字，然后使用任何 `init()` 参数调用资源命名。资源只能在定义它的范围内创建。这样可以防止任何人能够创建其他人定义的任意数量的资源对象。

```Cadence
let newHello <- create HelloAsset()
```

在这里我们使用运算符`<-`。代表移动操作。在涉及资源操作中，移动运算符取代赋值运算符`=`。为了明确分配资源，移动运算符`<-`当且仅当如下的情况才使用：

+ 初始化时，资源是常量或变量。
+ 资源在分配中移动到不同的变量。
+ 资源作为参数移动到函数。
+ 资源从函数中返回。

在资源移动后，之前的存放地址已经失效，对象移动到新的存放地址的上下文中。不允许常规的任务分配，因为常规的任务分配只会复制值。资源在同一时间仅能存在于一个位置，所以移动资源务必要显式的展示在代码中。

它使用 `AuthAccount.save` 函数将其存储在帐户存储中。

```Cadence
self.account.save(<-newHello, to: /storage/Hello)
```

合约可以使用关键字 `self` 引用其成员函数和字段。所有合约都可以访问部署它们的帐户的存储内容，并且可以使用 `self.account` 访问该 `AuthAccount` 对象。

`AuthAccount`（授权对象）有许多不同的方法用来和帐户的存储内容交互。您可以在语言参考-存储部分或词汇表中查看所有这些的文档。`save`方法保存一个对象到账户的存储中去。对象类型的类型参数包含在 `<>` 中以指示存储的对象是什么类型。这也可以推断出来参数是什么类型。

第一个参数是正在存储的对象，参数`to`代表了该对象应该存放在哪个路径下。路径应该是一个存储路径，也就是说，只有路径的域名为stroage才可以出现在`to`参数中。

如果给定路径下已经存储了一个对象，则程序中止。要记住，Cadence类型系统确保了资源不会发生意外的丢失。把资源移动到字段、数组、字典或存储时，存储位置可能已经包含该资源。Cadence要求开发人员处理这种已经存在资源的这种情况，以免因覆盖而意外丢失。这也是为什么我们在`newHello`中什么也不做也不能让函数执行完毕。并且解释了如果资源已经存在在指定的路径下，`save`会报错。

在这种情况下，这是我们使用所选帐户运行的第一笔交易，因此我们知道`/storage/Hello `处的存储位置是空的。在实际应用中，我们可能会对存储的位置执行必要的检查和操作，以确保我们不会因为意外覆盖而中止交易。

现在您已将资源存储在帐户中，您应该会看到该资源显示在编辑器下方的资源框中。此框指示所选帐户中存储了哪些资源，以及这些资源中字段的值。现在，您应该看到 `HelloAsset` 资源存储在帐户 `0x02` 的账户存储中，并且没有字段。

![img](https://storage.googleapis.com/flow-resources/documentation-assets/cadence-tuts/playground-resources.png)



### **与资源交互**

---

>打开名为`Load Hello `的交易
>
>`Load Hello`应该包含如下代码

```Cadence
import HelloWorld from 0x02

// This transaction calls the "hello" method on the HelloAsset object
// that is stored in the account's storage by removing that object
// from storage, calling the method, and then putting it back in storage

transaction {

    prepare(acct: AuthAccount) {

        // load the resource from storage, specifying the type to load it as
        // and the path where it is stored
        let helloResource <- acct.load<@HelloWorld.HelloAsset>(from: /storage/Hello)

        // We use optional chaining (?) because the value in storage
        // may or may not exist, and thus is considered optional.
        log(helloResource?.hello())

        // Put the resource back in storage at the same spot
        // We use the force-unwrap operator `!` to get the value
        // out of the optional. It aborts if the optional is nil
        acct.save(<-helloResource!, to: /storage/Hello)
    }
}
```

这笔交易从我们刚才部署的账户上导入了`HelloWorld`的定义，从账户存储中移走了`HelloAsset`对象，并且调用了存储在`HelloAsset`资源中的`hello()`函数。

为了把账户存储中的对象拿走，我们使用了`load`方法。

```Cadence
let helloResource <- acct.load<@HelloWorld.HelloAsset>(from: /storage/Hello)
```

如果没有对象存储在给定的路径下，函数会返回nil。当函数返回后，该路径下的存储里不会有该对象。

要加载的对象类型的类型参数包含在 `<>` 中。类型参数必须要显式表达出来，就像这里`@HelloWorld.HelloAsset`。（注意`@`表明这是一个资源）

路径`path`一定是一个账户存储路径，也就是只允许域名为` /storage/`。

接下来，我们调用 `hello` 函数并记录输出。

```Cadence
log(helloResource?.hello())
```

我们用 `?`因为存储中的值作为[备选值(optionals)](https://docs.onflow.org/cadence/language/values-and-types/#optionals)返回。备选值可以表示空值。备选值有两种情况：要么有指定类型的值，要么什么都没有（`nil`）。可选类型是使用 `?`后缀。

```Cadence
let newResource: HelloAsset?  // could either have a value of type `HelloAsset`
                              // or it could have a value of `nil`
```

备选值允许开发者更优雅地处理值为`nil`的情况。在这里，我们必须明确地考虑到我们通过 `load` 获得的 `helloResource` 对象为`nil`的可能性。在调用`hello`函数之前，使用`?`"扩展"备选值使用范围，但前提是该值不为零。如果值为 `nil`， `?`返回零。

因为在调用`hello`函数时`?`已经使用了，如果存储值不为`nil`函数调用才会发生。在这个例子中，`hello` 函数的结果将作为备选值返回。但是，如果存储的值为 `nil`，则不会发生函数调用并且结果为 `nil`。

接下来，我们再次使用 `save` 将对象放回存储中的同一位置：

```Cadence
acct.save(<-helloResource!, to: /storage/Hello)
```

请记住，`helloResource`为一个备选值，所以我们必须要处理值为`nil`的情况。我们使用强制解析（force-unwrap operator）(`!`)。如果它包含一个值，则此运算符获取备选值中的值，如果对象为 `nil`，则中止整个交易。这是一种处理备选值的风险更大的方式，但是如果您的程序曾经处于值为`nil`会破坏整个交易，那么强制解析（force-unwrap） 运算符是处理这种情况的不错选择。

可以参阅[Cadence备选值(Optionals In Cadence)](https://docs.onflow.org/cadence/language/values-and-types/#optionals)]来学习到更多用法。

>选择帐户 `0x02` 作为唯一的签名者。点击`Send`按钮提交交易。

您应该会看到如下内容：

```
"Hello, World!"
```



### **创建功能定位符和对存储资源的引用**

---

在这个栗子中，我们会创建一个链接去引用`HelloAsset`资源对象，然后使用这个引用去调用`hello`函数。此交易中发生的事情的详细说明位于交易代码下方，因此如果您感到费解，请继续阅读！

>打开名为`Create Link`的交易
>
>`Create Link`应该包含如下代码

`Create`

```Cadence
import HelloWorld from 0x02

// This transaction creates a new capability
// for the HelloAsset resource in storage
// and adds it to the account's public area.
//
// Other accounts and scripts can use this capability
// to create a reference to the private object to be able to
// access its fields and call its methods.

transaction {
  prepare(account: AuthAccount) {

    // Create a public capability by linking the capability to
    // a `target` object in account storage.
    // The capability allows access to the object through an
    // interface defined by the owner.
    // This does not check if the link is valid or if the target exists.
    // It just creates the capability.
    // The capability is created and stored at /public/Hello, and is
    // also returned from the function.
    let capability = account.link<&HelloWorld.HelloAsset>(/public/Hello, target: /storage/Hello)

    // Use the capability's borrow method to create a new reference
    // to the object that the capability links to
    // We use optional chaining "??" to get the value because
    // result of the borrow could fail, so it is an optional. 
    // If the optional is nil,
    // the panic will happen with a descriptive error message
    let helloReference = capability!.borrow()
      ?? panic("Could not borrow a reference to the hello capability")

    // Call the hello function using the reference 
    // to the HelloAsset resource.
    //
    log(helloReference.hello())
  }
}
```

>确保账户`0x02`仍然为账户签名者
>
>点击`Send`按钮来执行这笔交易

您应该可以看到`"Hello, World"`又一次出现在终端上。这是因为我们用**功能定位符(capability)**绑定了`HelloAsset`这个对象，存储在功能定位符中，路径为`/public/Hello`，并借用了一个引用，使用我们的引用来调用对象的 `hello` 方法。

功能定位符（Capabilities）有点类似于其他语言的指针。账户存储域名`/storage/`是私有的，但是用户仍然希望允许其他用户访问他们的私有对象的功能，而不能删除它们。***译注：这句话概述了功能定位符的作用***

在我们的栗子中，`HelloAsset`的所有者仍然希望想要让其他人去调用`hello`方法。这就是功能定位符做的事情。它们代表帐户存储中对象的链接，该对象具有创建链接时指定的类型。

要注意功能定位符只能允许访问字段和方法。但是并不允许复制，移动，或者直接修改源对象。

让我们分解一下这笔交易中发生的事情。

首先，我们在创建了一个功能定位符指向了`/stroage/`路径下的`HelloAsset`私有对象。

```Cadence
let capability = account.link<&HelloWorld.HelloAsset>(/public/Hello, target: /storage/Hello)
```

`HelloAsset`对象存储在`/stroage/Hello`，这个路径下只有账户所有者才能访问。他们想要互联网上的任何用户都可以调用`hello`方法，因此在`/public/Hello`创建了一个功能定位符。

为了创建一个功能定位符，我们使用了`AuthAccount.link`方法来用一个新的功能定位符来绑定指定对象。`<>` 中包含的类型是功能定位符代表的受限引用类型。该功能表示，从该功能定位符中借用引用的人只能访问 `<>` 中类型指定的字段和方法。

用符号`&`代表引用。在这里，功能定位符引用了`HelloAsset`对象，而且我们明确了限定`<&HelloWorld.HelloAsset>`该类型，这授予对 `HelloAsset` 对象中所有内容的访问权限。

`link`这个函数的第一个参数是您要存储功能的路径，`target`这个参数是要链接到的存储中对象的路径。

为了从功能定位符借用对象的引用，我们使用功能定位符的借用方法。

```Cadence
let helloReference = capability!.borrow()
    ?? panic("Could not borrow a reference to the hello capability")
```

我们在`link`函数中明确了这个方法在创建引用时候只能使用`<>`中的类型。在这里我们使用强制解析运算符(`!`)因为功能定位符是一个备选值(optional)。如果功能运算符为`nil`交易就会终止。在借用引用时，我们使用备选值来确保返回值，因为引用的借用可能会失败。如果目标存储槽是空的、已经被借用，或者如果请求的类型超出了功能运算符允许的范围，则引用可能为`nil`。我们用`panic`来包装错误消息，以便调用者可以更好地知道出了什么问题。

我们将此过程分为功能定位符和引用的原因是为了防止恶意行为者多次调用对象的重入bug。这些bug已经困扰其他的智能合约语言。一次只能存在一个对象引用，因此存储中的对象不可能存在这种类型的漏洞。

此外，对象的所有者可以通过移动功能定位符绑定的对象或使用 `unlink` 方法破坏链接来有效地撤销他们创建的功能定位符。如果被引用对象被移动或者链接被销毁，之前创建的功能定位符就变成无效的链接了。

最后，我们使用借用的引用调用 `hello()` 方法：

```Cadence
// Call the hello function using the reference to the HelloAsset resource
log(helloReference.hello())
```



### **执行脚本**

---

在Cadence中，脚本是非常简单的合约类型，它不能对区块链执行任何写入操作，只能读取帐户的状态。运行脚本无需任何账户授权。

为了运行脚本，您需写一个叫做`pub fun main()`的函数。你可以点击执行脚本按钮来去运行脚本。执行的结果会打印在终端。

>打开文件`Script1.cdc`
>
>`Script1.cdc`应该为如下代码：

`Script1.cdc`

```Cadence
import HelloWorld from 0x02

pub fun main() {

    // Cadence code can get an account's public account object
    // by using the getAccount() built-in function.
    let helloAccount = getAccount(0x02)

    // Get the public capability from the public path of the owner's account
    let helloCapability = helloAccount.getCapability<&HelloWorld.HelloAsset>(/public/Hello)

    // borrow a reference for the capability
    let helloReference = helloCapability.borrow()
        ?? panic("Could not borrow a reference to the hello capability")

    // The log built-in function logs its argument to stdout.
    //
    // Here we are using optional chaining to call the "hello"
    // method on the HelloAsset resource that is referenced
    // in the published area of the account.
    log(helloReference.hello())
}
```

该脚本用`getAccount`方法获取到了一个账户对象`PublicAccount`。

```Cadence
let helloAccount = getAccount(0x02)
```

公有账户对象`PublicAccount`是在互联网上对任何人都可以使用的对象，但只能访问一小部分功能，这些功能只能从帐户中的` /public/` 域名中读取。在此处查看有关帐户的更多信息：

https://docs.onflow.org/cadence/language/accounts/

然后，`hellAccount`这个对象获取功能定位符中绑定链接`Create Link` 。

```Cadence
// Get the public capability from the public path of the owner's account
let helloCapability = helloAccount.getCapability(/public/Hello)
```

为了获得存储在账户的功能运算符，我们使用`account.getCapability`函数。这个函数可以在`AuthAccount`和`PublicAccount`中使用。它在指定的路径上返回一个功能定位符，也具有指定的类型。它不检查目标是否存在，但如果能力无效，借用将失败。

脚本从功能定位符中借用了一个引用。

```Cadence
let helloReference = helloCapability.borrow()
```

然后，脚本使用引用调用 `hello` 函数并打印结果。

让我们执行脚本以查看它是否正确运行。

> 在开发者平台点击`Execute`按钮

![img](https://storage.googleapis.com/flow-resources/documentation-assets/cadence-tuts/playground-execute.png)

您应该看到类似这样的打印：

```Cadence
> "Hello, World"
> Result > "void"
```

做得好！您已经部署了您的第一个 Cadence 智能合约，并使用交易和脚本与之交互！

如果您仍然需要一些说明，这里有一些关于开发者平台的某些方面的提示。



### **账户**

---

当您打开开发者平台时，平台已经自动为您初始化了一定数量的可配置的默认账户。

在这个平台下，您可以通过选择屏幕左侧部分中该帐户的选项卡来选择帐户以编辑要部署的合同。该帐户对应的合约将显示在编辑器中，您可以在其中编辑并将其部署到区块链。

![img](https://storage.googleapis.com/flow-resources/documentation-assets/cadence-tuts/playground-accounts.png)

升级已经部署的合约是非常危险的，所以要小心！



### **交易**

---

智能合约部署后，你可以提交交易来与之进行交互。在屏幕左侧的交易选择部分，您可以选择不同的交易进行编辑和发送。在交易开启时，您可以选择一个或多个账户来签署交易。这是因为在Flow中，多个账户可以签署相同的合约，从而可以访问其私人存储。如果选择多个账户作为签名者，这需要在交易的签名中体现，以显示多个签名者：

```Cadence
// One signer

transaction {
    prepare(acct1: AuthAccount) {}
}

// Two signers

transaction {
    prepare(acct1: AuthAccount, acct2: AuthAccount) {}
}
```

如果您想要更多练习，您可以在新帐户上运行一些以前的交易，用来查找一些不同的交互和潜在的错误消息。



### **Flow 上的同质化代币**

---

现在您已经在 Flow 上编写并启动了您的第一个智能合约，您已经为更复杂的事情做好了准备！





## **3.同质化代币（Fungible Tokens）**

---

在这篇教程中，我们将部署、存储和转移同质化代币。

---

>在Flow开发者平台中打开本教程的入门代码：
>
>https://play.onflow.org/7c60f681-40c7-4a18-82de-b81b3b6c7915
>
>本教程将指导您采取各种操作来与此代码进行交互。

>需要您采取行动的说明总是包含在这样的标注框中。
>
>这些突出显示的操作是让代码运行所需的全部操作，但阅读其余部分对于理解语言的设计是必要的。

当今区块链上一些最受欢迎的合约类别是同质化代币。这些合约创建了同质化代币，可以转移给其他用户并作为货币消费（例如，以太坊上的 ERC-20）。

通常，中央分布式账本会跟踪用户的代币余额。通过 Cadence，我们使用新的资源范式来实现同质化的代币并避免使用中央分布式账本。



### **Flow网络代币**

---

在Flow，本地网络代币将使用类似于本教程中的智能合约作为普通的同质化代币智能合约来实现。将有特殊的合约和挂钩允许它用于交易执行支付和抵押，但除此之外，开发人员和用户将能够像对待网络中的任何其他代币一样对待和使用它！

我们将带您完成以下步骤，以熟悉同质化代币：

1. 将同质化代币合约部署到账户 `0x01`
2. 创建一个同质化代币对象并将其存储在您的帐户存储中。
3. 创建对您的代币的引用，其他人可以使用它来向您发送代币。
4. 以同样的方式设置另一个帐户。
5. 将代币从一个帐户转移到另一个帐户。
6. 使用脚本读取账户余额。

**在继续本教程之前，**我们建议您按照[Getting Started](https://docs.onflow.org/cadence/tutorial/01-first-steps/)和 [Hello, World!](https://docs.onflow.org/cadence/tutorial/02-hello-world/) 中的说明进行操作。以便学会这门语言的基础和了解开发者平台。



### **在Flow模拟器中的同质化代币**

---

>首先，您需要按照此链接打开带有预加载的同质化合约、交易和脚本的开发者平台：
>
>https://play.onflow.org/7c60f681-40c7-4a18-82de-b81b3b6c7915?type=account&id=0

>打开账户`0x01`标签页名为`ExampleToken.cdc`的文件。
>
>`ExampleToken.cdc`应该包含完整的代码和同质化代币，这个代码提供了在您的帐户中存储同质化代币
>
>以及转移和接受其他用户的代币的核心功能。

在 Cadence 中实现同质化代币所涉及的概念一开始可能很难理解。有关此功能和代码的深入说明，请继续阅读下一节。

或者，如果您想立即部署它并在开发者平台中使用它，您可以跳到本教程的[与同质化代币交互](https://docs.onflow.org/cadence/tutorial/03-fungible-tokens/#interacting-with-the-fungible-token-in-the-flow-playground)部分。



### **深入探索同质化代币**

---

Flow 实现同质化代币的方式与其他编程语言不同：

+ 所有权是去中心化的，不依赖于中央账本
+ 漏洞和漏洞为用户带来的风险更小，攻击者的机会更少
+ 没有整数下溢或上溢的风险
+ 资产不可复制，极难丢失、被盗、毁坏
+ 代码可以组合
+ 规则可以是不变的
+ 代码不会未经授权账户允许公开



### **去中心化所有权**

---

与中心分布式记账系统不同，对于资产所有权来说，通过一种新的模式Flow把所有权与每个账户相关联。下面的示例展示了 Solidity 如何实现同质化代币，为简洁起见，仅显示了用于存储和传输代币的代码。

`ERC20.sol`

```
contract ERC20 {
    mapping (address => uint256) private _balances;

    function _transfer(address sender, address recipient, uint256 amount) {
        // ensure the sender has a valid balance
        require(_balances[sender] >= amount);

        // subtract the amount from the senders ledger balance
        _balances[sender] = _balances[sender] - amount;

        // add the amount to the recipient’s ledger balance
        _balances[recipient] = _balances[recipient] + amount
    }
}
```

如您所见，Solidity 使用中央分类帐系统去操作同质化代币。有一个管理代币状态的合约，每次用户想要用他们的代币做任何事情时，他们都必须与中央 ERC20 合约进行交互，调用其函数来更新他们的余额。该合约处理所有功能的访问控制，所有正确性检查都调用自己，并为所有用户实施这些规则。

与中心分布式记账不同，Flow 利用一些不同的概念为智能合约开发人员和用户提供更好的安全性、保密性和可读性。在本节中，我们将通过一个同质化代币示例展示如何使用 Flow 的资源、接口和其他功能。



### **直观的资源所有权**

---

**资源**（**Resources**）在Cadence是一个很重要的概念，这代表了线性模型。一个资源是一个复合类型，它有自己定义的字段和函数，类似于结构体。区别在于资源对象有特殊的规则来防止它们被复制或丢失。资源是资产所有权的新范式。每个账户在其账户存储中拥有一个资源对象，记录他们拥有的代币数量，而不是在中央账本智能合约中代表代币所有权。这样，当用户想要相互交易时，他们可以进行点对点交易，而无需与中央代币合约进行交互。为了相互转移代币，他们在自己的资源对象和其他用户的资源上调用`transfer`转移函数（或等效的东西），而不是中央`transfer`转移函数。

这种方法简化了访问控制，因为中央合约不必检查函数调用的发送者，大多数函数调用发生在存储在账户的资源对象中，并且每个用户控制谁能够调用其帐户中资源的函数。这个概念称为基于功能定位符（Capability-based security）的安全性，将在后面的部分中详细解释。

这种方法也防止了潜在的bug。在具有中央合约包含的所有逻辑的 Solidity 中，一个漏洞可能会影响所有参与合约的用户。但在Cadence中，如果一个资源有逻辑上的bug，攻击者必须单独利用每个代币持有者帐户中的漏洞，这比中央分布式记账系统要复杂和耗时得多。

下面是同质化代币资源示例。拥有这些代币的每个用户都会将此资源存储在他们的帐户中。请务必记住，每个帐户仅存储 Vault 资源的副本，而不是整个 ExampleToken 合约的副本。 ExampleToken 合约只需要存储在管理代币定义的初始帐户中。

`Token.cdc`

```Cadence
pub resource Vault: Provider, Receiver {

    // Balance of a user's Vault
    // we use unsigned integers for balances because they do not require the
    // concept of a negative number
    pub var balance: UFix64

    init(balance: UFix64) {
        self.balance = balance
    }

    pub fun withdraw(amount: UFix64): @Vault {
        self.balance = self.balance - amount
        return <-create Vault(balance: amount)
    }

    pub fun deposit(from: @Vault) {
        self.balance = self.balance + from.balance
        destroy from
    }
}
```

这段代码很容易理解并且很有代表性。因为它展示了一个代币资源的工作原理。每一个代币资源对象都有其余额并且和相应的函数绑定（例如：`deposit`, `withdraw`, 等等）。当用户想要使用这些代币时，系统会帮他们在账户存储中实例化出来一个零余额的资源副本。Cadence要求只运行一次的初始化函数 `init` 必须初始化所有成员变量。

```Candece
// Balance of a user's Vault
// we use unsigned fixed-point integers for balances because they do not require the
// concept of a negative number and allow for more clear precision
pub var balance: UFix64

init(balance: UFix64) {
    self.balance = balance
}
```

然后，存款（deposit）函数可用于任何账户转移代币。

```Cadence
pub fun deposit(from: @Vault) {
    self.balance = self.balance + from.balance
    destroy from
}
```

当账户想要发送给不同账户一些代币，发送方首先会调用他们自己的提款（withdraw）函数，这会从他们的资源余额中减去代币，并临时创建一个新的资源对象来保持这个余额。然后发送方调用接收方的存款（deposit）函数，这也符合表面上将资源实例移动到另一个帐户，将其添加到他们的余额中，然后销毁使用的资源。资源需要销毁，因为 Cadence 对资源交互执行了严格的限制。资源永远不能挂在一段代码中。它要么需要明确销毁，要么存储在帐户的存储中。

与资源交互时，您使用`@ `符号和特殊的“移动运算符”`<-`。

```Cadence
pub fun withdraw(amount: UInt64): @Vault {
```

`@`符号需要为字段、参数或返回值指定资源类型。移动运算符 `<-` 明确地表明，当在赋值、参数或返回值中使用资源时，它被移动到新位置，旧位置无效。这确保了同一时间资源只能存在在一处位置。

如果资源从账户的存储中移出，则需要将其移动到账户的存储中或明确销毁。

```Cadence
destroy from
```

此规则可确保通常代表实际价值的资源不会因编码错误而丢失。

您会注意到算术运算并未明确防止上溢或下溢。

```Cadence
self.balance = self.balance - amount
```

在 Solidity 中，这可能存在整数上溢或下溢的风险，但 Cadence 具有内置的上溢和下溢保护，因此不存在风险。在这个例子中我们也使用了无符号整数，所以钱包的余额不能低于 0。

此外，帐户存储中包含代币资源类型副本的限制可确保资金不会因发送到错误地址而丢失。如果地址没有导入正确的资源类型，交易将恢复，确保发送到错误地址的交易不会丢失。

在提款（withdraw）函数里这行创建了一个新的`Vault`并且在调用过程中指定了参数名`balance`。

```Cadence
return <-create Vault(balance: amount)
```

另一个作用是Cadence必须提高代码的可读性。所有函数调用都需要指定它们发送的参数的名称，除非开发人员明确覆盖了函数声明中的要求。



### **确保公有安全性：功能定位符的安全性**

---

Cadence 的另一个重要特性是它对功能运算符安全性的支持。这一特性确保，虽然提款功能是公开的，但除了预期用户和他们批准的人之外，没有人可以从他们的金库中提取代币。

Cadence的安全性模型确保了在账户存储中的对象只能被所有者授权的账户才能访问。如果一个用户想让另一个用户访问他们存储的对象，他们可以链接一个公有功能定位符，这就像一个“API”，允许其他人调用他们对象上的指定函数。

一个帐户只有在拥有明确允许他们访问这些字段和方法的对象的功能定位符时，才有权访问不同帐户中对象的字段和方法。只有对象的所有者才能为其创建引用。因此，当用户在其帐户中创建 Vault 时，他们仅发布对存款功能和余额的引用。提款函数可以保持隐藏状态，作为只有所有者才能调用的函数。

正如您看到的，这消除了出于访问控制目的检查 `msg.sender` 的需要，因为此功能由协议和类型检查器处理。如果您不是对象的所有者或没有所有者创建的对它的有效引用，您根本无法访问该对象！



### **使用接口来保护实现**

---

下一个在Cadence中重要的概念是智能合约的设计，使用前置条件和后置条件来记录并以编程方式断言由程序段引起的状态更改。这些条件在接口中指定，这些接口强制执行有关如何定义类型和行为的规则。它们可以以不可变的方式存储在链上，以便某些代码可以导入和实现它们以确保它们符合某些标准。

这是我们上面定义的 `Vault` 资源接口的示例。

`Interfaces.cdc`

```Cadence
// Interface that enforces the requirements for withdrawing
// tokens from the implementing type
//
pub resource interface Provider {
    pub fun withdraw(amount: UFix64): @Vault {
        post {
            result.balance == amount:
                "Withdrawal amount must be the same as the balance of the withdrawn Vault"
        }
    }
}
// Interface that enforces the requirements for depositing
// tokens into the implementing type
//
pub resource interface Receiver {

    // There aren't any meaningful requirements for only a deposit function
    // but this still shows that the deposit function is required in an implementation.
    pub fun deposit(from: @Vault)
}
```

在我们的例子中，`Vault`资源将会实现这两个接口。这些接口确保资源实现中存在特定字段和函数，并且这些功能在执行之前和/或之后满足某些条件。这些接口可以存储在链上并导入到其他合约或资源中，以便这些要求由不易受人为错误影响的不可变事实来源强制执行。

还可以看到，函数和字段的定义都有关键字`pub`。我们需要显式地定义这些字段或函数为公有，因为在Cadence中所有字段和函数都默认是私有的，这也就意味着只有本地范围能访问它们。用户必须明确公开他们拥有的一部分类型。这有助于防止类型无意中公开代码。



### **在Flow开发者平台上与同质化代币交互**

---

现在您已经了解了同质化代币的工作原理，我们可以将其部署到您的帐户并发送一些交易以与之交互。

>按照本页顶部的链接，确保您已在开发者平台中打开了同质化代币的模板。您应该打开帐户 
>
>`0x01`，并且应该看到下面的代码。

`ExampleToken.cdc`

```Cadence
pub contract ExampleToken {

    // Total supply of all tokens in existence.
    pub var totalSupply: UFix64

    // Provider
    //
    // Interface that enforces the requirements for withdrawing
    // tokens from the implementing type.
    //
    // We don't enforce requirements on self.balance here because
    // it leaves open the possibility of creating custom providers
    // that don't necessarily need their own balance.
    //
    pub resource interface Provider {

        // withdraw
        //
        // Function that subtracts tokens from the owner's Vault
        // and returns a Vault resource (@Vault) with the removed tokens.
        //
        // The function's access level is public, but this isn't a problem
        // because even the public functions are not fully public at first.
        // anyone in the network can call them, but only if the owner grants
        // them access by publishing a resource that exposes the withdraw
        // function.
        //
        pub fun withdraw(amount: UFix64): @Vault {
            post {
                // `result` refers to the return value of the function
                result.balance == UFix64(amount):
                    "Withdrawal amount must be the same as the balance of the withdrawn Vault"
            }
        }
    }

    // Receiver
    //
    // Interface that enforces the requirements for depositing
    // tokens into the implementing type.
    //
    // We don't include a condition that checks the balance because
    // we want to give users the ability to make custom Receivers that
    // can do custom things with the tokens, like split them up and
    // send them to different places.
    //
    pub resource interface Receiver {
        // deposit
        //
        // Function that can be called to deposit tokens
        // into the implementing resource type
        //
        pub fun deposit(from: @Vault)
    }

    // Balance
    //
    // Interface that specifies a public `balance` field for the vault
    //
    pub resource interface Balance {
        pub var balance: UFix64
    }

    // Vault
    //
    // Each user stores an instance of only the Vault in their storage
    // The functions in the Vault and governed by the pre and post conditions
    // in the interfaces when they are called.
    // The checks happen at runtime whenever a function is called.
    //
    // Resources can only be created in the context of the contract that they
    // are defined in, so there is no way for a malicious user to create Vaults
    // out of thin air. A special Minter resource needs to be defined to mint
    // new tokens.
    //
    pub resource Vault: Provider, Receiver, Balance {

        // keeps track of the total balance of the account's tokens
        pub var balance: UFix64

        // initialize the balance at resource creation time
        init(balance: UFix64) {
            self.balance = balance
        }

        // withdraw
        //
        // Function that takes an integer amount as an argument
        // and withdraws that amount from the Vault.
        //
        // It creates a new temporary Vault that is used to hold
        // the money that is being transferred. It returns the newly
        // created Vault to the context that called so it can be deposited
        // elsewhere.
        //
        pub fun withdraw(amount: UFix64): @Vault {
            self.balance = self.balance - amount
            return <-create Vault(balance: amount)
        }

        // deposit
        //
        // Function that takes a Vault object as an argument and adds
        // its balance to the balance of the owners Vault.
        //
        // It is allowed to destroy the sent Vault because the Vault
        // was a temporary holder of the tokens. The Vault's balance has
        // been consumed and therefore can be destroyed.
        pub fun deposit(from: @Vault) {
            self.balance = self.balance + from.balance
            destroy from
        }
    }

    // createEmptyVault
    //
    // Function that creates a new Vault with a balance of zero
    // and returns it to the calling context. A user must call this function
    // and store the returned Vault in their storage in order to allow their
    // account to be able to receive deposits of this token type.
    //
    pub fun createEmptyVault(): @Vault {
        return <-create Vault(balance: UFix64(0))
    }

    // VaultMinter
    //
    // Resource object that an admin can control to mint new tokens
    pub resource VaultMinter {

        // Function that mints new tokens and deposits into an account's vault
        // using their `Receiver` reference.
        // We say `&AnyResource{Receiver}` to say that the recipient can be any resource
        // as long as it implements the Receiver interface
        pub fun mintTokens(amount: UFix64, recipient: &AnyResource{Receiver}) {
            ExampleToken.totalSupply = ExampleToken.totalSupply + UFix64(amount)
            recipient.deposit(from: <-create Vault(balance: amount))
        }
    }

    // The init function for the contract. All fields in the contract must
    // be initialized at deployment. This is just an example of what
    // an implementation could do in the init function. The numbers are arbitrary.
    init() {
        self.totalSupply = UFix64(30)

        // create the Vault with the initial balance and put it in storage
        // account.save saves an object to the specified `to` path
        // The path is a literal path that consists of a domain and identifier
        // The domain must be `storage`, `private`, or `public`
        // the identifier can be any name
        self.account.save(<-create Vault(balance: UFix64(30)), to: /storage/MainVault)

        // Create a new VaultMinter resource and store it in account storage
        self.account.save(<-create VaultMinter(), to: /storage/MainMinter)
    }
}

```

> 单击编辑器右下角的 `Deploy` 按钮部署代码。

此部署将同质化代币合约存储在您的活动账户（账户 `0x01`）中，以便可以将其导入到交易中。

合约的 `init` 函数在合约创建时运行，之后不再运行。在我们的示例中，此函数存储初始余额为 30 的 `Vault` 对象实例，并存储可用于铸造新代币的 `VaultMinter` 对象。

```Cadence
// create the Vault with the initial balance and put it in storage
// account.save saves an object to the specified `to` path
// The path is a literal path that consists of a domain and identifier
// The domain must be `storage`, `private`, or `public`
// the identifier can be any name
self.account.save(<-create Vault(balance: UFix64(30)), to: /storage/MainVault)
```

此行将新的 `@Vault` 对象保存到存储中。帐户存储是由路径索引，路径由域名和标识符组成。 `/domain/identifier`。路径只允许三个域：

+ `storage`：存放所有对象的地方。只能由帐户所有者访问。
+ `private`：将存储链接（也称为功能定位符）绑定存储的对象。只能由帐户所有者访问。
+ `public`：存储链接指向存储的对象：网络中的任何人都可以访问。

合约可以使用 `self.account` 访问其部署到的帐户的私有 `AuthAccount` 对象。该对象具有可以通过多种方式修改存储的方法。有关它可以调用的所有方法的列表，请参阅[帐户](https://docs.onflow.org/cadence/language/accounts)文档。

在这一行中，我们调用 `save` 方法将对象存储在 storage 中。第一个参数是要存储的值，第二个参数是存储值的路径。`sava`路径必须在 `/storage` 域名中。

我们还以相同的方式将 `VaultMinter` 对象存储到下一行的 `/storage/` 中：

```Cadence
self.account.save(<-create VaultMinter(), to: /storage/MainMinter)
```

您还应该看到 `ExampleToken.Vault` 和 `ExampleToken.VaultMinter` 资源对象存储在帐户存储中。这将显示在屏幕底部的资源框中。

您现在已准备好运行使用同质化代币的交易！



### **创建、存储和公开对 Vault 的功能定位符和引用**

---

功能定位符（Capabilities）就像其他语言中的指针。它们是帐户存储中对象的链接，可用于读取字段或调用它们引用的对象上的函数。他们不能直接移动或修改对象。

面对许多不同的场景你可以对你的同质化代币库存（Vault）去创建功能定位符。你可能想要一个简单的方式从交易中的任何位置调用你的`Vault`方法。您还可以发送一个仅在您的 `Vault` 中公开提款功能的功能定位符，以便其他人可以为您转移代币。你也可以有一个只公开 Balance 接口，以便其他人可以检查你拥有多少代币。也可以有一个函数将 `Vault` 的功能定位符作为参数，借用对该功能的引用，对该引用进行单个函数调用，然后完成并销毁该引用。

我们已经在 `mintTokens` 函数的 `Vault` 资源中使用了这种模式，如下所示：

```Cadence
// Function that mints new tokens and deposits into an account's vault
// using their `Receiver` capability.
// We say `&AnyResource{Receiver}` to say that the recipient can be any resource
// as long as it implements the Receiver interface
pub fun mintTokens(amount: UFix64, recipient: Capability<&AnyResource{Receiver}>) {
    let recipientRef = recipient.borrow()
        ?? panic("Could not borrow a receiver reference to the vault")

    ExampleToken.totalSupply = ExampleToken.totalSupply + UFix64(amount)
    recipientRef.deposit(from: <-create Vault(balance: amount))
}
```

该函数获取对任何实现 `Receiver` 接口的资源的功能定位符，从中借用一个引用，并使用该引用调用该资源的存款函数。

让我们为您的 `Vault` 创建功能定位符，以便单独的帐户可以向您发送代币。

> 打开名为 `Create Link` 的交易。
>
> `Create Link` 应包含以下代码，用于创建对存储的 Vault 的引用：

`Create`

```Cadence
import ExampleToken from 0x01

// This transaction creates a capability
// that is linked to the account's token vault.
// The capability is restricted to the fields in the `Receiver` interface,
// so it can only be used to deposit funds into the account.
transaction {
  prepare(acct: AuthAccount) {

    // Create a link to the Vault in storage that is restricted to the
    // fields and functions in `Receiver` and `Balance` interfaces,
    // this only exposes the balance field
    // and deposit function of the underlying vault.
    //
    acct.link<&ExampleToken.Vault{ExampleToken.Receiver, ExampleToken.Balance}>(/public/MainReceiver, target: /storage/MainVault)

    log("Public Receiver reference created!")
  }

  post {
    // Check that the capabilities were created correctly
    // by getting the public capability and checking
    // that it points to a valid `Vault` object
    // that implements the `Receiver` interface
    getAccount(0x01).getCapability<&ExampleToken.Vault{ExampleToken.Receiver}>(/public/MainReceiver)
                    .check():
                    "Vault Receiver Reference was not created correctly"
    }
}
```

为了使用功能定位符，我们必须创建一个链接绑定存储里的对象。然后可以从功能定位符创建引用，并且不能存储引用。这些引用需要在交易执行结束时丢弃。此限制是为了防止重入攻击，即恶意用户在原始执行完成之前一遍又一遍地调用同一函数的攻击。一次只允许一个对象的一个引用可以防止对存储的对象进行攻击。

为了创建功能定位符，我们使用`link`函数。

```Cadence
// Create a link to the Vault in storage that is restricted to the
// fields and functions in `Receiver` and `Balance` interfaces,
// this only exposes the balance field
// and deposit function of the underlying vault.
//
acct.link<&ExampleToken.Vault{ExampleToken.Receiver, ExampleToken.Balance}>.
(/public/MainReceiver, target: /storage/MainVault)
```

`link`函数创建了一个新的功能定位符其路径保存在第一个参数里，绑定的目标在第二个`target`参数里。链接中的类型显式用`<>`去进行类型检查。我们使用`&ExampleToken.Vault{ExampleToken.Receiver, ExampleToken.Balance}`去表达这个链接可以是任何资源，只要它实现并被转换为 Receiver 接口。这是描述引用的通用格式。首先有一个符号`&`后面紧跟具体类型，然后是大括号里的接口，以确保它是实现该接口的引用，并且只包含该接口中指定的字段。

我们把功能定位符放到`/public/MainReceiver`路径下，因为我们想要这个路径公有可读。网络中的任何人都可以通过帐户的 `PublicAccount` 对象访问帐户的`public`域名，该对象是使用 `getAccount(address)` 函数获取的。

接下来是交易的后置条件`post`阶段。

```Cadence
post {
// Check that the capabilities were created correctly
// by getting the public capability and checking
// that it points to a valid `Vault` object
// that implements the `Receiver` interface
getAccount(0x01).getCapability(/public/MainReceiver)
                .check<&ExampleToken.Vault{ExampleToken.Receiver}>():
                "Vault Receiver Reference was not created correctly"
}
```

后置条件`post`阶段是为了确保在交易执行后满足某些条件。在这里，我们从其公有路径获取功能定位符并调用其`check`函数以确保该功能定位符包含指向存储中指定类型的有效链接绑定的有效对象。

>选择帐户 `0x01` 作为唯一的签名者。
>
>点击`Send`发送按钮提交交易。
>
>此交易会创建一个对您的 `Vault` 的新公有引用，并检查它是否已正确创建。



### **代币转移给其他用户**

---

现在，我们想要运行一笔交易去发送给`0x02`账户10个代币。我们将通过调用账户 `0x01` 的 Vault 上的提款`withdraw`函数来做到这一点，它创建一个临时的 Vault 对象来移动代币，然后通过调用他们的存款`deposit`函数将这些代币存入账户 `0x02` 的账户。

在这里，我们遇到了 Cadence 的另一个安全特性。我们拥有的代币需要一个在你账户中的`Vault`对象来存储，所以如果任何人想要发送给并没有想要准备接收代币的人发送代币，这笔交易就会失败。以这种方式，如果用户在发送代币时不小心输入了错误的帐户地址，Cadence 会取消这笔交易来保护用户。

帐户 `0x02` 尚未设置为接收代币，因此我们现在将这样做：

> 打开交易`Setup Account`。
>
> 选择帐户` 0x02` 作为唯一的签名者。
>
> 单击`Send`按钮设置帐户 `0x02`，以便它可以接收代币。

`Setup`

```Cadence
import ExampleToken from 0x01

// This transaction configures an account to store and receive tokens defined by
// the ExampleToken contract.
transaction {
  prepare(acct: AuthAccount) {
    // Create a new empty Vault object
    let vaultA <- ExampleToken.createEmptyVault()

    // Store the vault in the account storage
    acct.save<@ExampleToken.Vault>(<-vaultA, to: /storage/MainVault)

    log("Empty Vault stored")

    // Create a public Receiver capability to the Vault
    let ReceiverRef = acct.link<&ExampleToken.Vault{ExampleToken.Receiver, ExampleToken.Balance}>(/public/MainReceiver, target: /storage/MainVault)

    log("References created")
  }

  post {
    // Check that the capabilities were created correctly
    getAccount(0x02).getCapability(/public/MainReceiver)
                    .check<&ExampleToken.Vault{ExampleToken.Receiver}>():
                    "Vault Receiver Reference was not created correctly"
  }
}
```

在这里，我们执行帐户 `0x01` 为设置其 `Vault` 所做的相同操作，但都是在一笔交易中完成的。账户 `0x02` 已准备好开始赚钱！如您所见，当我们为账户 `0x02` 创建 `Vault` 时，我们必须通过调用 `createEmptyVault()` 函数创建一个余额为零的账户。资源的创建仅限于定义它的合约，因此通过这种方式，同质化代币智能合约可以确保没有人能够凭空创建新的代币。

作为 ExampleToken 合约初始部署过程的一部分，账户 `0x01` 创建了一个 `VaultMinter` 对象。通过使用这个对象，拥有它的账户可以铸造新的代币。现在，帐户 `0x01` 拥有它，因此它拥有铸造新代币的唯一权力。我们本可以在合约中定义一个 `mintTokens` 函数，但是我们必须检查函数调用的发送者以确保他们被授权，这不是执行访问控制的推荐方式。

正如我们之前所解释的，资源模型加上功能定位符的安全性为我们处理这种访问控制，作为一种内置的语言结构，而不是必须在代码中定义。如果账户 `0x01` 想要授权另一个账户铸造代币，他们可以将 `VaultMinter` 对象移动到另一个账户，或者给另一个账户一个私有功能给单个 `VaultMinter`。或者，如果他们不希望在部署后进行铸造，他们会在合约初始化时简单地铸造所有代币，甚至不将 `VaultMinter` 包含在合约中。

在下一笔交易中，账户 `0x01` 将铸造 30 个新代币并将它们存入账户 `0x02` 新创建的 Vault对象中。

>仅选择帐户 `0x01` 作为签名者，并发送 `Mint Tokens`为帐户 `0x02` 铸造 30 个代币。
>
>`Mint Tokens` 应包含以下代码。

`Mint`

```Cadence
import ExampleToken from 0x01

// This transaction mints tokens and deposits them into account 2's vault
transaction {

  // Local variable for storing the reference to the minter resource
  let mintingRef: &ExampleToken.VaultMinter

  // Local variable for storing the reference to the Vault of
  // the account that will receive the newly minted tokens
  var receiverRef: &ExampleToken.Vault{ExampleToken.Receiver}

  prepare(acct: AuthAccount) {
    // Borrow a reference to the stored, private minter resource
    self.mintingRef = acct.borrow<&ExampleToken.VaultMinter>(from: /storage/MainMinter)
        ?? panic("Could not borrow a reference to the minter")

    // Get the public account object for account 0x02
    let recipient = getAccount(0x02)

    // Get the public receiver capability
    let cap = recipient.getCapability(/public/MainReceiver)

    // Borrow a reference from the capability
    self.receiverRef = cap.borrow<&ExampleToken.Vault{ExampleToken.Receiver}>()
        ?? panic("Could not borrow a reference to the receiver")
}

  execute {
      // Mint 30 tokens and deposit them into the recipient's Vault
      self.mintingRef.mintTokens(amount: UFix64(30), recipient: self.receiverRef)

      log("30 tokens minted and deposited to account 0x02")
  }
}
```

在这笔交易中，我们利用跨越不同阶段的本地变量来完成交易，这在我们的示例中尚属首次。我们在准备阶段之外声明 `mintingRef` 和`receiverRef` 变量，但必须在准备阶段初始化它们。然后我们可以在交易的后置条件阶段使用它们。

除了从功能定位符中借用引用之外，您还将在此交易中看到，您还可以直接从存储中的对象借用引用。

```Cadence
// Borrow a reference to the stored, private minter resource
self.mintingRef = acct.borrow<&ExampleToken.VaultMinter>(from: /storage/MainMinter)
    ?? panic("Could not borrow a reference to the minter")
```

在这里，我们将借用指定为 `VaultMinter` 引用，并将引用指向 `/storage/MainMinter`。他的引用是作为备选项借用的，因此我们使用 nil 合并运算符 (`??`) 来确保该值不是 nil。如果值为nil，交易将执行`??`。代码就是错误的，所以它将返回状态并打印错误消息。

您可以使用 `getAccount()` 内置函数来获取任何帐户的公有帐户对象。公共帐户对象允许您从存储公有功能定位符的帐户的`public`域名中获取功能定位符。

我们使用 `getCapability` 函数从公共路径获取公共能力，然后使用功能定位符上的`borrow`借用函数从中获取引用，类型为 `ExampleToken.Vault{ExampleToken.Receiver}`。

```Cadence
// Get the public receiver capability
let cap = recipient.getCapability(/public/MainReceiver)

// Borrow a reference from the capability
self.receiverRef = cap.borrow<&ExampleToken.Vault{ExampleToken.Receiver}>()
        ?? panic("Could not borrow a reference to the receiver")
```

在执行阶段，我们简单地使用引用来铸造 30 个代币并将它们存入账户 `0x02` 的 `Vault` 中。



### **检查帐户余额**

---

现在，帐户 `0x01` 和帐户 `0x02` 的存储中都应该有一个 `Vault` 对象，该对象的余额为 30 个代币。他们都应该在他们的 `/public/` 域名中存储一个接收`Receiver`功能，链接到他们存储中的 `Vault`对象。

![img](https://storage.googleapis.com/flow-resources/documentation-assets/cadence-tuts/account-balances.png)

除非专门配置过接受这些代币，否则帐户无法接收任何代币类型。因此，很难意外地将代币发送到错误的地址。但是，如果您在新帐户中设置 `Vault` 时出错，您将无法向其发送代币。

让我们运行一个脚本来确保我们正确设置了库存(vaults)。

您可以使用脚本访问帐户的公开状态。脚本未由任何帐户签名且无法修改状态。

在这个例子中，我们将查询每个账户的库存(vaults)余额。下面将在模拟器中打印出每个帐户的余额。

>在脚本栏下打开名为 `Script1.cdc` 的脚本
>
>`Script1.cdc` 应包含以下代码：

`Script1.cdc`

```Cadence
import ExampleToken from 0x01

// This script reads the Vault balances of two accounts.
pub fun main() {
    // Get the accounts' public account objects
    let acct1 = getAccount(0x01)
    let acct2 = getAccount(0x02)

    // Get references to the account's receivers
    // by getting their public capability
    // and borrowing a reference from the capability
    let acct1ReceiverRef = acct1.getCapability<&ExampleToken.Vault{ExampleToken.Balance}>(/public/MainReceiver)
        .borrow()
        ?? panic("Could not borrow a reference to the acct1 receiver")

    let acct2ReceiverRef = acct2.getCapability<&ExampleToken.Vault{ExampleToken.Balance}>(/public/MainReceiver)
        .borrow()
        ?? panic("Could not borrow a reference to the acct2 receiver")

    // Read and log balance fields
    log("Account 1 Balance")
    log(acct1ReceiverRef.balance)
    log("Account 2 Balance")
    log(acct2ReceiverRef.balance)
}
```

>单击执行按钮执行 `Script1.cdc`。
>
>执行后保证了以下几点：

+ 账户 `0x01` 的余额为 30
+ 账户 `0x02` 的余额为 30

如果正确，您应该看到以下几行：

```
"Account 1 Balance"
30
"Account 2 Balance"
30
Result > "void"
```

如果出现错误，这可能意味着您之前错过了一步，可能需要从头开始。

要重新启动开发者平台，请关闭当前会话并打开教程顶部的链接。

现在我们有两个账户，每个账户都有一个 `Vault`，我们可以看看他们是如何相互转移代币的！

>打开名为 `Transfer Tokens` 的交易。
>
>选择账户 `0x02` 作为签名者并发送交易。
>
>`Transfer Tokens` 应包含以下用于向另一个用户发送令牌的代码：

`Transfer`

```Cadence
import ExampleToken from 0x01

// This transaction is a template for a transaction that
// could be used by anyone to send tokens to another account
// that owns a Vault
transaction {

  // Temporary Vault object that holds the balance that is being transferred
  var temporaryVault: @ExampleToken.Vault

  prepare(acct: AuthAccount) {
    // withdraw tokens from your vault by borrowing a reference to it
    // and calling the withdraw function with that reference
    let vaultRef = acct.borrow<&ExampleToken.Vault>(from: /storage/MainVault)
        ?? panic("Could not borrow a reference to the owner's vault")

    self.temporaryVault <- vaultRef.withdraw(amount: UFix64(10))
  }

  execute {
    // get the recipient's public account object
    let recipient = getAccount(0x01)

    // get the recipient's Receiver reference to their Vault
    // by borrowing the reference from the public capability
    let receiverRef = recipient.getCapability(/public/MainReceiver)
                      .borrow<&ExampleToken.Vault{ExampleToken.Receiver}>()
                      ?? panic("Could not borrow a reference to the receiver")

    // deposit your tokens to their Vault
    receiverRef.deposit(from: <-self.temporaryVault)

    log("Transfer succeeded!")
  }
}
```

在这个例子中，签名者从他们的 `Vault` 中提取代币，这会创建并返回一个 `balance=10` 的临时 `Vault` 资源对象，用于转移代币。在执行阶段。交易使用他们的存款`deposit`函数将该资源移动到另一个用户的`Vault`。临时`Vault`在其余额添加到接收者的`Vault`后被销毁。

您可能想知道为什么我们必须使用两个函数调用来完成一次代币传输，而不是一次完成。因为这是资源在 Cadence 中的工作原理。在基于分布式账本的模型中，您只需调用 transfer，它只会更新分布式账本，但在 Cadence 中，代币的位置很重要，因此大多数代币转移情况不仅仅是直接账户到账户的转账。大多数情况下，代币将首先用于不同的目的，例如购买东西，这需要在存入帐户存储之前单独发送和验证 `Vault`。将两者分开还使我们能够利用静态验证帐户的哪些部分可以在交易的准备部分进行修改，这将帮助用户在从应用程序获取馈送交易时高枕无忧。

> 再次执行 `Script1.cdc` 。

如果正确，您应该看到以下几行，表明账户 `0x01` 的余额为 40，账户 `0x02` 的余额为 20：

```
"Account 1 Balance"
40
"Account 2 Balance"
20
Result > "void"
```

您现在知道如何在 Cadence 和 Flow 中使用基本的同质化代币了！

从这里开始，您可以尝试通过以下方式扩展同质化代币的功能：

+ 生成代币的源头
+ 可以存入的托管（但只有在余额达到某个点时才能提取）
+ 用于铸造新代币资源的函数！



### **非同质化代币**

---

既然您已经了解了同质化代币如何在 Flow 上工作，您就可以开始使用非同质化代币了！



## **4.非同质化代币（Non-Fungible Tokens）**

---

在本教程中，我们将部署、存储和传输**非同质化代币 (NFT)**。

---

>在 Flow开发者平台中打开本教程的入门代码：
>
>https://play.onflow.org/b60aa97d-d030-476e-9490-24f3a6f90161
>
>本教程将要求您采取操作来与此代码进行交互。

>需要您采取行动的说明总是包含在这样的标注框中。
>
>这些突出显示的操作是让代码运行所需的全部操作，但阅读其余部分对于理解语言的设计是必要的。

NFT 是区块链技术不可或缺的一部分。我们需要 NFT 来表示独特且不可分割的资产（例如 CryptoKitties！或 Top Shot Moments！）。

Cadence 不像大多数智能合约语言那样在中央分布式帐本中表示，而是将每个 NFT 表示为用户存储在帐户中的资源对象。

我们将带您完成以下步骤以熟悉 NFT：

1. 部署 NFT 合约和类型定义。
2. 创建 NFT 对象并将其存储在您的帐户存储中。
3. 创建一个 NFT 集合对象以在您的账户中存储多个 NFT。
4. 创建一个 `NFTMinter` 并使用它来创建 NFT。
5. 创建对您的集合的引用，其他人可以使用它来向您发送代币。
6. 以同样的方式设置另一个帐户。
7. 将 NFT 从一个账户转移到另一个账户。
8. 使用脚本查看每个帐户的集合中存储了哪些 NFT。

**在继续本教程之前，**我们强烈建议您按照入门、Hello, World! 和 Fungible Tokens 中的说明学习如何使用 开发者平台工具并学习 Cadence 的基础知识。我们将在这里再次介绍一些概念，但不是全部。



### **在Flow模拟器上的非同质化代币**

---

在 Cadence 中，每个 NFT 都由一个带有整数 ID 的资源表示。资源是表示 NFT 的完美类型，因为资源具有所有权规则，并且由类型系统执行。它们只能有一个所有者，不能被复制，也不能被意外或恶意丢失或复制。这些保护措施确保所有者知道他们的 NFT 是安全的，并且可以代表具有实际价值的资产。

NFT 通常也由某种元数据表示，例如名称或图片。从历史上看，这些元数据的大部分都存储在链外，链上代币仅包含指向链外元数据的 URL 或类似内容。在 Flow 中，这是可能的，与代币相关的所有元数据都可以存储在链上。这超出了该教程涉及的范围。这类标准仍然逐步改善并且您也可以查看在flow NFT repository[相关问题](https://github.com/onflow/flow-nft/issues/9)参加讨论。

当flow用户想要与他人进行交易，他们可以调用在每个账户下资源定义的方法，无需与中央NFT合约交互，去实现点对点的交易。



### **添加 NFT 到您的帐户**

---

>首先，您需要按照此链接打开带有预加载的非同质化代币合约、交易和脚本的开发者平台：
>
>https://play.onflow.org/b60aa97d-d030-476e-9490-24f3a6f90161

>打开账户 `0x01` 以查看 `NFTv1.cdc`。 `NFTv1.cdc` 应包含以下代码：

`NFTv1.cdc`

```Cadence
pub contract ExampleNFT {

    // Declare the NFT resource type
    pub resource NFT {
        // The unique ID that differentiates each NFT
        pub let id: UInt64

        // String mapping to hold metadata
        pub var metadata: {String: String}

        // Initialize both fields in the init function
        init(initID: UInt64) {
            self.id = initID
            self.metadata = {}
        }
    }

    // Create a single new NFT and save it to account storage
    init() {
        self.account.save<@NFT>(<-create NFT(initID: 1), to: /storage/NFT1)
    }
}
```

在这个合约中，NFT 是一个具有整数 ID 和元数据字段的资源。

这与同质化代币不同，因为同质化代币只是一个金库，当代币被提取或存入时，其余额可能会发生变化。每个 NFT 资源都有唯一的 ID，因此它们不能组合或复制，除非智能合约允许。

这种设计的另一个独特之处是每个 NFT 都可以包含自己的元数据。在这个例子中，我们使用了一个简单的字符串（`String`）到字符串（`String`）的映射，但您可以想象一个更丰富的版本，它可以允许存储复杂的文件格式和其他此类数据。

一个 NFT 甚至可以拥有其他 NFT！此示例在后面的教程中显示。

在合约的 `init` 函数中，我们创建一个新的 NFT 对象并将其移动到帐户存储中。

```
// put it in storage
self.account.save<@NFT>(<-create NFT(initID: 1), to: /storage/NFT1)
```

在这里，我们合约部署到的帐户上的 `AuthAccount` 对象并调用其 `save` 方法，指定 `@NFT` 作为其保存的类型。我们还在同一行中创建了 NFT，并将其作为第一个参数传递给 `save`。我们将它保存到 `/storage` 域名中，在那里存储对象。

> 单击编辑器右下角的部署按钮部署 `NFTv1`。

您现在应该在您的帐户中有一个 NFT。让我们运行一个交易去检查。

> 打开 `NFT Exists` 交易，选择账户 `0x01` 作为唯一签名者，并发送交易。
>
> `NFT Exists` 应如下所示：

`NFT`

```
import ExampleNFT from 0x01

// This transaction checks if an NFT exists in the storage of the given account
// by trying to borrow from it
transaction {
    prepare(acct: AuthAccount) {
        if acct.borrow<&ExampleNFT.NFT>(from: /storage/NFT1) != nil {
            log("The token exists!")
        } else {
            log("No token found!")
        }
    }
}
```

在这里，我们试图直接从存储中的 NFT 借用引用。如果对象存在，则借用成功，备选引用不会为 `nil`，但如果借用失败，则可选引用为 `nil`。

您应该会看到`“The token exists!”`的内容。

干得好！您的账户中有第一个 NFT。



### **在一个集合中存储多个 NFT**

---

我们可以将我们的 NFT 放在存储的顶层，但是如果您有很多 NFT，那么组织所有 NFT 可能会开始变得混乱。这种方法不具有可扩展性，但我们可以通过使用可以容纳任意数量 NFT 的数据结构来克服这个问题。我们可以通过数组或字典来实现这一点，但这些类型相对不透明。相反，我们可以使用资源作为我们的 NFT 集合，以启用更复杂的方式与我们的 NFT 交互。

>打开账户`0x22`可以看到`NFTv2.cdc`。
>
>单击编辑器右下角的 Deploy 按钮部署合约。
>
>`NFTv2.cdc` 应包含以下代码。它包含 `NFTv1.cdc` 中已有的内容以及合约正文中的其他资源声明。

`NFTv2.cdc`

```Cadence
// NFTv2.cdc
//
// This is a complete version of the ExampleNFT contract
// that includes withdraw and deposit functionality, as well as a
// collection resource that can be used to bundle NFTs together.
//
// It also includes a definition for the Minter resource,
// which can be used by admins to mint new NFTs.
//
// Learn more about non-fungible tokens in this tutorial: https://docs.onflow.org/docs/non-fungible-tokens

pub contract ExampleNFT {

    // Declare the NFT resource type
    pub resource NFT {
        // The unique ID that differentiates each NFT
        pub let id: UInt64

        // Initialize the field in the init function
        init(initID: UInt64) {
            self.id = initID
        }
    }

    // We define this interface purely as a way to allow users
    // to create public, restricted references to their NFT Collection.
    // They would use this to only expose the deposit, getIDs,
    // and idExists fields in their Collection
    pub resource interface NFTReceiver {

        pub fun deposit(token: @NFT)

        pub fun getIDs(): [UInt64]

        pub fun idExists(id: UInt64): Bool
    }

    // The definition of the Collection resource that
    // holds the NFTs that a user owns
    pub resource Collection: NFTReceiver {
        // dictionary of NFT conforming tokens
        // NFT is a resource type with an `UInt64` ID field
        pub var ownedNFTs: @{UInt64: NFT}

        // Initialize the NFTs field to an empty collection
        init () {
            self.ownedNFTs <- {}
        }

        // withdraw 
        //
        // Function that removes an NFT from the collection 
        // and moves it to the calling context
        pub fun withdraw(withdrawID: UInt64): @NFT {
            // If the NFT isn't found, the transaction panics and reverts
            let token <- self.ownedNFTs.remove(key: withdrawID)!

            return <-token
        }

        // deposit 
        //
        // Function that takes a NFT as an argument and 
        // adds it to the collections dictionary
        pub fun deposit(token: @NFT) {
            // add the new token to the dictionary with a force assignment
            // if there is already a value at that key, it will fail and revert
            self.ownedNFTs[token.id] <-! token
        }

        // idExists checks to see if a NFT 
        // with the given ID exists in the collection
        pub fun idExists(id: UInt64): Bool {
            return self.ownedNFTs[id] != nil
        }

        // getIDs returns an array of the IDs that are in the collection
        pub fun getIDs(): [UInt64] {
            return self.ownedNFTs.keys
        }

        destroy() {
            destroy self.ownedNFTs
        }
    }

    // creates a new empty Collection resource and returns it 
    pub fun createEmptyCollection(): @Collection {
        return <- create Collection()
    }

    // NFTMinter
    //
    // Resource that would be owned by an admin or by a smart contract 
    // that allows them to mint new NFTs when needed
    pub resource NFTMinter {

        // the ID that is used to mint NFTs
        // it is only incremented so that NFT ids remain
        // unique. It also keeps track of the total number of NFTs
        // in existence
        pub var idCount: UInt64

        init() {
            self.idCount = 1
        }

        // mintNFT 
        //
        // Function that mints a new NFT with a new ID
        // and returns it to the caller
        pub fun mintNFT(): @NFT {

            // create a new NFT
            var newNFT <- create NFT(initID: self.idCount)

            // change the id so that each ID is unique
            self.idCount = self.idCount + 1 as UInt64
            
            return <-newNFT
        }
    }

	init() {
		// store an empty NFT Collection in account storage
        self.account.save(<-self.createEmptyCollection(), to: /storage/NFTCollection)

        // publish a reference to the Collection in storage
        self.account.link<&{NFTReceiver}>(/public/NFTReceiver, target: /storage/NFTCollection)

        // store a minter resource in account storage
        self.account.save(<-create NFTMinter(), to: /storage/NFTMinter)
	}
}
```

拥有一个或多个 `ExampleNFT` 的任何用户都将在其帐户中存储 `ExampleNFT.Collection` 资源的实例。该集合将所有 `NFT` 存储在一个字典中，该字典将整数 ID 映射到 NFT，类似于 `Vault` 资源在 `balance` 字段中存储所有代币的方式。另一个相似之处是每个`collection`都有存款`withdraw`和取款`deposit`函数。这些功能允许用户遵循同一个模式：即先从`collection`取走代币，然后存到其他的账户的`collection`。

当用户想要在他们账户存储NFTs，他们会在`ExampleNFT`智能合约中调用`createEmptyCollection`函数实例化一个空的`Collection`。这将返回一个空的 `Collection` 对象，他们可以将其存储在他们的帐户存储中。

我们在此示例中使用了一些新功能，因此让我们逐一介绍它们。



### **字典**

---

该资源使用了[字典：一个可变的、无序的键值关联集合](https://docs.onflow.org/cadence/language/values-and-types/#dictionaries)。

```
pub var ownedNFTs: @{Int: NFT}
```

在字典中，所有键必须具有相同的类型，并且所有值必须具有相同的类型。在这种情况下，我们将整数 ID 映射到 `NFT` 资源对象。字典定义中通常不用 `@`符号来进行类型检查，是因为`ownedNFTs`映射存储资源，整个字段也必须成为资源类型，这就是为什么该字段有 `@` 符号表示它是一种资源类型。这意味着适用于资源的所有规则都适用于这种类型。

如果使用 `destroy` 命令销毁 NFT 集合(collection)资源，它需要知道如何处理它存储在字典中的资源。这也解释了在调用`destroy`函数时，要销毁的该资源内部必须包含一个`destroy`函数以供运行。这个`destroy`函数必须显式销毁资源或将它们移动到其他地方。在这个例子中，我们销毁它们。

```Cadence
destroy() {
    destroy self.ownedNFTs
}
```

当集合被创建时，`init`构造函数运行并且显式初始化所有成员变量。这有助于防止某些智能合约中出现未初始化字段可能导致错误的问题。在此之后，`init`构造函数将永远无法再次运行。在这里，我们使用空字典将字典初始化为资源类型。

```
init () {
  self.ownedNFTs <- {}
}
```

字典的另一个特性是能够使用内置的`keys`键函数获取字典键的数组。

```Cadence
// getIDs returns an array of the IDs that are in the collection
pub fun getIDs(): [UInt64] {
    return self.ownedNFTs.keys
}
```

这用于遍历字典或仅查看存储内容的列表。如您所见，可变长度数组类型是通过将成员类型括在方括号中来声明的。



### **资源可以拥有其他资源**

---

`NFTv2.cdc` 中的这个 NFT Collection 示例说明了一个重要特性：资源可以拥有其他资源。

在示例中，用户可以将一个 NFT 转移给另一个用户。此外，由于集合`Collection`明确拥有其中的 NFT，所有者只需转移单个集合`Collection`即可一次转移所有 NFT。

这是一个很重要的特性，因为它支持许多其他用例。除了允许轻松的批量传输之外，这意味着如果一个独特的 NFT 想要拥有另一个独特的 NFT，例如拥有帽子配件的 CryptoKitty，Kitty 会将帽子存储在自己的存储中并有效地拥有它。帽子属于存放它的 CryptoKitty，帽子可以单独转让，也可以与拥有它的 CryptoKitty 一起转让。

无法为存储在其他资源中的资源创建功能定位符，但可以为引用创建功能定位符。属于自己的资源可以控制功能定位符，因此也能控制外部调用对存储资源的访问类型。



### **限制对 NFT 集合的访问**

---

在NFT集合中，所有的函数和字段都是公有的，但是我们又不想让每一个人在网上都可以调用我们的`withdraw`函数。这也是加入Cadence的双层访问控制的来由。Cadence使用功能定位符提供的安全性，这意味着任何给定的对象，如果用户满足以下任一条件，则允许访问该对象的字段或方法：

+ 是对象的所有者
+ 拥有对该字段或方法的有效引用（注意，引用只能从功能定位符创建，功能定位符只能由对象的所有者创建）

当用户想要在账户存储他们的NFT`Collection`时，默认情况下不是对外开放的。除了所有者之外，任何人都无法访问用户的帐户存储对象。为了给外部账户对`deposit`存款函数、`getIDs` 函数和 `idExists` 函数的读取访问权限，所有者将使用仅包含这些字段的接口创建引用：

```Cadence
pub resource interface NFTReceiver {

    pub fun deposit(token: @NFT)

    pub fun getIDs(): [UInt64]

    pub fun idExists(id: UInt64): Bool
}
```

然后，使用该接口，他们将创建一个指向存储中对象的链接，指定该链接仅包含 NFTReceiver 接口中的函数。这个链接就是功能定位符。从功能定位符那里，所有者可以使用该功能做任何他们想做的事情：他们可以将其作为参数传递给一次性使用的函数，或者他们可以将其放入其帐户的 `/public/` 域名中，以便任何人都可以访问它。如果用户尝试使用此功能来调用`withdraw`提款函数，它将无法工作，因为它不存在于接口或引用中。

在 `init`构造函数中 `NFTv2.cdc` 的第 132 行中可以看到链接和功能定位符的创建：

```Cadence
// publish a reference to the Collection in storage
self.account.link<&{NFTReceiver}>(/public/NFTReceiver, target: /storage/NFTCollection)
```

上面的`link`函数指定了功能定位符类型为`&AnyResource{NFTReceiver}`只对公开这些字段和函数。然后链接存储在任何人都可以访问的 `/public/` 中。该链接针对我们之前创建的 `/storage/NFTCollection`。

现在，用户在他们的帐户`/storage`中拥有一个 NFT 集合，以及其他人可以使用它来查看他们拥有哪些 NFT 并向他们发送 NFT 。

让我们通过运行脚本来确认这是真的！



### **运行脚本**

---

Cadence 中的脚本是简单的交易，无需任何帐户权限即可运行，仅从区块链读取信息。

> 打开名为 `Print 0x02 NFTs` 的脚本文件。`Print 0x02 NFT` 应包含以下代码：

```Cadence
import ExampleNFT from 0x02

// Print the NFTs owned by account 0x02.
pub fun main() {
    // Get the public account object for account 0x02
    let nftOwner = getAccount(0x02)

    // Find the public Receiver capability for their Collection
    let capability = nftOwner.getCapability<&{ExampleNFT.NFTReceiver}>(/public/NFTReceiver)

    // borrow a reference from the capability
    let receiverRef = capability.borrow()
            ?? panic("Could not borrow receiver reference")

    // Log the NFTs that they own as an array of IDs
    log("Account 2 NFTs")
    log(receiverRef.getIDs())
}
```

>单击编辑器框右下角的“执行”按钮，执行打印 `0x02 NFT`。
>
>此脚本打印帐户 `0x02` 拥有的 NFT 列表。

因为账户 `0x02` 当前在其集合中没有任何NFT集合，它只会打印一个空数组：

```
"Account 2 NFTs"
[]
Result > "void"
```

如果脚本无法执行，则可能意味着 NFT 集合未正确存储在帐户 `0x02` 中。如果您遇到问题，请确保您选择帐户 `0x02` 作为签名者帐户，并且您正确执行了前面的步骤。



### **以管理员身份铸造和分发代币**

---

创建 NFT 的一种方法是让管理员铸造新代币并将其发送给用户。大多数人会通过拥有 NFT Minter 资源来实现这一点。此资源的所有者可以铸造代币，或者如果他们想赋予其他用户和合约铸造代币的能力，则所有者可以提供仅公开 `mintNFT` 功能以利用功能定位符安全性。无需像基于分布式帐本模型那样明确检查交易的发送者！

让我们使用 NFT Minter 来铸造一些代币。

如果你回顾 `NFTv2.cdc` 中的 `ExampleNFT` 合约，你会看到它定义了另一个资源，`NFTMinter`。这是一个简单的示例，说明具有铸造权限的管理员将拥有铸造新 NFT 的资产。这只是一个单一的函数来创建 NFT 并且只包含一个递增的整数字段，用于为 NFT 分配唯一的 ID。

您应该看到在帐户 `0x02` 的帐户存储中存储了一个 `ExampleNFT.NFTMinter` 资源。这显示在编辑器下方的`Resources`框中。

现在我们可以使用我们存储的 `NFTMinter` 来铸造一个新的 NFT 并将其存入账户 `0x02` 的集合中。

>打开名为 `Mint NFT` 的文件。选择账户 `0x02` 作为唯一的签名者并发送交易。
>
>该交易将铸造的 NFT 存入账户所有者的 NFT 集合中：

`Mint`

```Cadence
import ExampleNFT from 0x02

// This transaction allows the Minter account to mint an NFT
// and deposit it into its collection.

transaction {

    // The reference to the collection that will be receiving the NFT
    let receiverRef: &{ExampleNFT.NFTReceiver}

    // The reference to the Minter resource stored in account storage
    let minterRef: &ExampleNFT.NFTMinter

    prepare(acct: AuthAccount) {
        // Get the owner's collection capability and borrow a reference
        self.receiverRef = acct.getCapability<&{ExampleNFT.NFTReceiver}>(/public/NFTReceiver)
            .borrow()
            ?? panic("Could not borrow receiver reference")

        // Borrow a capability for the NFTMinter in storage
        self.minterRef = acct.borrow<&ExampleNFT.NFTMinter>(from: /storage/NFTMinter)
            ?? panic("Could not borrow minter reference")
    }

    execute {
        // Use the minter reference to mint an NFT, which deposits
        // the NFT into the collection that is sent as a parameter.
        let newNFT <- self.minterRef.mintNFT()

        self.receiverRef.deposit(token: <-newNFT)

        log("NFT Minted and deposited to Account 2's Collection")
    }
}
```

>重新打开 `Print 0x02 NFT` 并执行脚本。这将打印帐户 `0x02` 拥有的 NFT 列表。

`Print`

```Cadence
import ExampleNFT from 0x02

// Print the NFTs owned by account 0x02.
pub fun main() {
    // Get the public account object for account 0x02
    let nftOwner = getAccount(0x02)

    // Find the public Receiver capability for their Collection
    let capability = nftOwner.getCapability<&{ExampleNFT.NFTReceiver}>(/public/NFTReceiver)

    // borrow a reference from the capability
    let receiverRef = capability.borrow()
            ?? panic("Could not borrow the receiver reference")

    // Log the NFTs that they own as an array of IDs
    log("Account 2 NFTs")
    log(receiverRef.getIDs())
}

```

您应该看到帐户 `0x02` 拥有 `id=1` 的 NFT

```
"Account 2 NFTs"
[1]
```



### **转移 NFT**

---

在我们能够将 NFT 转移到另一个帐户之前，我们需要使用他们自己的 NFTCollection 设置该帐户，以便他们能够接收 NFT。

> 打开名为 `Setup Account` 的文件并提交交易，使用账户 `0x01` 作为唯一的签名者。

`Setup`

```Cadence
import ExampleNFT from 0x02

// This transaction configures a user's account
// to use the NFT contract by creating a new empty collection,
// storing it in their account storage, and publishing a capability
transaction {
  prepare(acct: AuthAccount) {

    // Create a new empty collection
    let collection <- ExampleNFT.createEmptyCollection()

    // store the empty NFT Collection in account storage
    acct.save<@ExampleNFT.Collection>(<-collection, to: /storage/NFTCollection)

    log("Collection created for account 1")

    // create a public capability for the Collection
    acct.link<&{ExampleNFT.NFTReceiver}>(/public/NFTReceiver, target: /storage/NFTCollection)

    log("Capability created")
  }
}
```

帐户 `0x01` 现在应该有一个空的`Collection`集合资源存储在其帐户存储中。它还在其 `/public/` 域名中创建并存储了集合的功能定位符。

>打开名为 `Transfer` 的文件，选择账户 `0x02` 作为唯一签名者，然后发送交易。
>
>此交易将代币从帐户 `0x02` 转移到帐户 `0x01`。

`Transfer.cdc`

```Cadence
import ExampleNFT from 0x02

// This transaction transfers an NFT from one user's collection
// to another user's collection.
transaction {

    // The field that will hold the NFT as it is being
    // transferred to the other account
    let transferToken: @ExampleNFT.NFT

    prepare(acct: AuthAccount) {

        // Borrow a reference from the stored collection
        let collectionRef = acct.borrow<&ExampleNFT.Collection>(from: /storage/NFTCollection)
            ?? panic("Could not borrow a reference to the owner's collection")

        // Call the withdraw function on the sender's Collection
        // to move the NFT out of the collection
        self.transferToken <- collectionRef.withdraw(withdrawID: 1)
    }

    execute {
        // Get the recipient's public account object
        let recipient = getAccount(0x01)

        // Get the Collection reference for the receiver
        // getting the public capability and borrowing a reference from it
        let receiverRef = recipient.getCapability<&{ExampleNFT.NFTReceiver}>(/public/NFTReceiver)
            .borrow()
            ?? panic("Could not borrow receiver reference")

        // Deposit the NFT in the receivers collection
        receiverRef.deposit(token: <-self.transferToken)

        log("NFT ID 1 transferred from account 2 to account 1")
    }
}
```

现在我们可以检查两个帐户的集合以确保帐户 `0x01` 拥有令牌而帐户 `0x02` 没有任何东西。

> 执行脚本`Print all NFTs`以查看每个帐户中的代币：

`Script2.cdc`

```Cadence
import ExampleNFT from 0x02

// Print the NFTs owned by accounts 0x01 and 0x02.
pub fun main() {

    // Get both public account objects
    let account1 = getAccount(0x01)
      let account2 = getAccount(0x02)

    // Find the public Receiver capability for their Collections
    let acct1Capability = account1.getCapability<&{ExampleNFT.NFTReceiver}>(/public/NFTReceiver)
    let acct2Capability = account2.getCapability<&{ExampleNFT.NFTReceiver}>(/public/NFTReceiver)

    // borrow references from the capabilities
    let receiver1Ref = acct1Capability.borrow()
        ?? panic("Could not borrow account 1 receiver reference")
    let receiver2Ref = acct2Capability.borrow()
        ?? panic("Could not borrow account 2 receiver reference")

    // Print both collections as arrays of IDs
    log("Account 1 NFTs")
    log(receiver1Ref.getIDs())

    log("Account 2 NFTs")
    log(receiver2Ref.getIDs())
}
```

您应该在输出中看到类似的内容：

```
"Account 1 NFTs"
[1]
"Account 2 NFTs"
[]
```

账户 `0x01` 有一个 `ID=1` 的 NFT，账户 `0x02` 没有。这表明 NFT 从账户 `0x02` 转移到账户 `0x01`。

恭喜，您现在拥有一个有效的 NFT！



### **把NFT都放在一起**

---

这只是 NFT 如何在 Flow 上工作的一个基本示例。有关官方 [Flow NFT 标准](https://github.com/onflow/flow-nft)及其示例实现的信息，请参阅 Flow NFT 标准存储库。



### **创建Flow市场**

---

现在您有了一个可用的 NFT，您可以尝试自行扩展其功能，或者您可以学习如何创建一个同时使用可替代代币和 NFT 的市场。





## **5.安装市场**

---

>在 Flow Playground 中打开本教程的入门代码：
>
>https://play.onflow.org/46ad1d6d-3ee2-40d4-adef-bfbad33f9846
>
>本教程将要求您采取各种操作来与此代码进行交互。

如果您已经完成了 Marketplace 教程，请转到[复合资源：Kitty Hats](https://docs.onflow.org/cadence/tutorial/07-resources-compose/)。

本指南将帮助您快速将开发者平台设置为完成（Marketplace）市场教程所需的知识。市场教程使用同质化代币和非同质化代币合约来允许用户使用同质化代币买卖 NFT。

帐户的操作与您在开发者平台中完成同质化代币和非同质化代币教程一样。需要您继续跟着[复合智能合约：市场](https://docs.onflow.org/cadence/tutorial/06-marketplace-compose/)来学习。

1. 打开账户 `0x01`。确保同质化代币教程中 `FungibleToken.cdc` 中的同质化代币定义在此帐户中。
2. 将 Fungible Token代码部署到账户 `0x01`。
3. 通过从帐户选择菜单中选择帐户 `0x02` 切换到帐户 `0x02`。
4. 确保您在帐户 `0x02` 中的非同质化代币教程中的 `NFTv2.cdc` 中有 NFT 定义。
5. 将 NFT 代码部署到账户 `0x02`。
6. 在事务标签栏运行事务 3。这是名为 `SetupAccount1Transaction.cdc` 文件。使用帐户 `0x01` 作为设置帐户 `0x01` 帐户的唯一签名者。

`SetupAccount1Transaction.cdc`

```Cadence
import FungibleToken from 0x01
import NonFungibleToken from 0x02

// This transaction sets up account 0x01 for the marketplace tutorial
// by publishing a Vault reference and creating an empty NFT Collection.
transaction {
  prepare(acct: AuthAccount) {
    // Create a public Receiver capability to the Vault
    acct.link<&FungibleToken.Vault{FungibleToken.Receiver, FungibleToken.Balance}>
             (/public/MainReceiver, target: /storage/MainVault)

    log("Created Vault references")

    // store an empty NFT Collection in account storage
    acct.save<@NonFungibleToken.Collection>(<-NonFungibleToken.createEmptyCollection(), to: /storage/NFTCollection)

    // publish a capability to the Collection in storage
    acct.link<&{NonFungibleToken.NFTReceiver}>(/public/NFTReceiver, target: /storage/NFTCollection)

    log("Created a new empty collection and published a reference")
  }
}
```

7. 在事务标签栏运行事务4 。这是 `SetupAccount2Transaction.cdc` 文件。使用帐户 `0x02` 作为设置帐户 `0x02` 帐户的唯一签名者。

`SetupAccount2Transaction.cdc`

```Cadence

import FungibleToken from 0x01
import NonFungibleToken from 0x02

// This transaction adds an empty Vault to account 0x02
// and mints an NFT with id=1 that is deposited into
// the NFT collection on account 0x01.
transaction {

  // Private reference to this account's minter resource
  let minterRef: &NonFungibleToken.NFTMinter

  prepare(acct: AuthAccount) {
    // create a new vault instance with an initial balance of 30
    let vaultA <- FungibleToken.createEmptyVault()

    // Store the vault in the account storage
    acct.save<@FungibleToken.Vault>(<-vaultA, to: /storage/MainVault)

    // Create a public Receiver capability to the Vault
    let ReceiverRef = acct.link<&FungibleToken.Vault{FungibleToken.Receiver, FungibleToken.Balance}>(/public/MainReceiver, target: /storage/MainVault)

    log("Created a Vault and published a reference")

    // Borrow a reference for the NFTMinter in storage
    self.minterRef = acct.borrow<&NonFungibleToken.NFTMinter>(from: /storage/NFTMinter)
            ?? panic("Could not borrow an nft minter reference")
  }
  execute {
    // Get the recipient's public account object
    let recipient = getAccount(0x01)

    // Get the Collection reference for the receiver
    // getting the public capability and borrowing a reference from it
    let receiverRef = recipient.getCapability<&{NonFungibleToken.NFTReceiver}>(/public/NFTReceiver)
        .borrow()
        ?? panic("Could not borrow an nft receiver reference")

    // Mint an NFT and deposit it into account 0x01's collection
    self.minterRef.mintNFT(recipient: receiverRef)

    log("New NFT minted for account 1")
  }
}
```

8. 在事务标签栏运行事务 5 。这是名为 `SetupAccount1TransactionMinting.cdc` 文件。使用账户 `0x01` 作为唯一的签名者为账户 1 和 2 铸造同质化代币。

`SetupAccount1TransactionMinting.cdc`

```Cadence
import FungibleToken from 0x01
import NonFungibleToken from 0x02

// This transaction mints tokens for both accounts using
// the minter stored on account 0x01.
transaction {

    // Public Vault Receiver References for both accounts
    let acct1Ref: &AnyResource{FungibleToken.Receiver}
    let acct2Ref: &AnyResource{FungibleToken.Receiver}

    // Private minter references for this account to mint tokens
    let minterRef: &FungibleToken.VaultMinter

    prepare(acct: AuthAccount) {
        // Get the public object for account 0x02
        let account2 = getAccount(0x02)

        // Retrieve public Vault Receiver references for both accounts
        self.acct1Ref = acct.getCapability<&FungibleToken.Vault{FungibleToken.Receiver}>(/public/MainReceiver)
            .borrow()
            ?? panic("Could not borrow acct1 vault receiver reference")
        self.acct2Ref = account2.getCapability<&FungibleToken.Vault{FungibleToken.Receiver}>(/public/MainReceiver)
            .borrow()
            ?? panic("Could not borrow acct2 vault receiver reference")

        // Get the stored Minter reference for account 0x01
        self.minterRef = acct.borrow<&FungibleToken.VaultMinter>(from: /storage/MainMinter)
            ?? panic("Could not borrow vault minter reference")
    }

    execute {
        // Mint tokens for both accounts
        self.minterRef.mintTokens(amount: 20.0, recipient: self.acct2Ref)
        self.minterRef.mintTokens(amount: 10.0, recipient: self.acct1Ref)

        log("Minted new fungible tokens for account 1 and 2")
    }
}
```

9. 运行脚本 1 中的脚本 `CheckSetupScript.cdc` 文件以确保一切都已设置。

`CheckSetupScript.cdc`

```Cadence
import FungibleToken from 0x01
import NonFungibleToken from 0x02

// This script checks that the accounts are set up correctly for the marketplace tutorial.
//
// Account 0x01: Vault Balance = 40, NFT.id = 1
// Account 0x02: Vault Balance = 20, No NFTs
pub fun main() {
    // Get the accounts' public account objects
    let acct1 = getAccount(0x01)
    let acct2 = getAccount(0x02)

    // Get references to the account's receivers
    // by getting their public capability
    // and borrowing a reference from the capability
    let acct1ReceiverRef = acct1.getCapability<&FungibleToken.Vault{FungibleToken.Balance}>(/public/MainReceiver)
        .borrow<&FungibleToken.Vault{FungibleToken.Balance}>()
        ?? panic("Could not borrow acct1 vault receiver reference")
    let acct2ReceiverRef = acct2.getCapability<&FungibleToken.Vault{FungibleToken.Balance}>(/public/MainReceiver)
        .borrow()
        ?? panic("Could not borrow acct2 vault receiver reference")

    // Log the Vault balance of both accounts and ensure they are
    // the correct numbers.
    // Account 0x01 should have 40.
    // Account 0x02 should have 20.
    log("Account 1 Balance")
    log(acct1ReceiverRef.balance)
    log("Account 2 Balance")
    log(acct2ReceiverRef.balance)

    // verify that the balances are correct
    if acct1ReceiverRef.balance != 40.0 || acct2ReceiverRef.balance != 20.0 {
        panic("Wrong balances!")
    }

    // Find the public Receiver capability for their Collections
    let acct1Capability = acct1.getCapability<&{NonFungibleToken.NFTReceiver}>(/public/NFTReceiver)
    let acct2Capability = acct2.getCapability<&{NonFungibleToken.NFTReceiver}>(/public/NFTReceiver)

    // borrow references from the capabilities
    let nft1Ref = acct1Capability.borrow()
        ?? panic("Could not borrow acct1 nft receiver reference")
    let nft2Ref = acct2Capability.borrow()
        ?? panic("Could not borrow acct2 nft receiver reference")

    // Print both collections as arrays of IDs
    log("Account 1 NFTs")
    log(nft1Ref.getIDs())

    log("Account 2 NFTs")
    log(nft2Ref.getIDs())

    // verify that the collections are correct
    if nft1Ref.getIDs()[0] != 1 as UInt64 || nft2Ref.getIDs().length != 0 {
        panic("Wrong Collections!")
    }
}
```

10. 如果没有脚本抛出panic异常，你应该看到类似这样的输出

```
"Account 1 Vault Balance"
40
"Account 2 Vault Balance"
20
"Account 1 NFTs"
[1]
"Account 2 NFTs"
[]
```

现在您的开发者平台处于正确状态，您就可以继续学习下一个教程了。

您无需为市场教程打开新的开发者平台。你可以继续使用这个。





## **6.市场**

---

在本教程中，我们将创建一个市场，使用我们在之前的教程中学到的同质化和非同质化的代币 (NFT) 合约。

---

> 在 Flow开发者平台中打开本教程的入门代码：
>
> https://play.onflow.org/46ad1d6d-3ee2-40d4-adef-bfbad33f9846
>
> 本教程将要求您采取各种操作来与此代码进行交互。

> 需要您采取行动的说明总是包含在这样的标注框中。
>
> 这些突出显示的操作是让代码运行所需的全部操作，但阅读其余部分对于理解语言的设计是必要的。

市场是区块链技术和智能合约最活跃的应用。市面有NFTs存在时，用户希望能够使用他们的同质化代币买卖。

现在有了同质化和非同质化代币的标准，我们可以建立一个同时使用两者的市场。这被称为**模块化**：开发人员能够利用共享资源（例如代码或用户库）并将它们用作新应用程序的构建块。Flow 旨在实现模块化，因为它使开发人员能够事半功倍，从而实现快速创新。

为了创建一个市场，我们需要将同质化和非同质化代币的功能整合到一个单一的合约中，让用户可以控制他们的资金和资产。为了实现这一点，我们将带您完成以下步骤来创建一个模块化的智能合约并应用到市场：

1. 确保您的可替代代币和不可替代代币合约已正确部署和设置。
2. 将市场类型声明部署到账户 `0x03`。
3. 创建一个市场对象并将其存储在您的帐户中，将 NFT 出售并为您的售卖发布公有功能定位符。
4. 使用不同的帐户从销售中购买 NFT。
5. 运行脚本以验证是否已购买 NFT。

**在继续本教程之前**，您需要完成 [同质化代币](https://docs.onflow.org/cadence/tutorial/03-fungible-tokens/)和 [非同质化代币](https://docs.onflow.org/cadence/tutorial/04-non-fungible-tokens/) 教程以了解此智能合约的代码块是如何工作的。



### **设计市场**

---

实现市场的一种方法是拥有一个中心智能合约，用户将他们的 NFT 和价格存入其中，并且让任何人过来并能够以该价格购买NFT。这种方法是合理的，它使过程集中。我们希望用户在尝试出售 NFT 的同时能够保持对他们出售的 NFT 的所有权。

与采取中心化方法不同，每个用户可以在他们自己账户上售卖物品。然后，用户可以提供一个引用挂在中心化应用市场去售卖自己的物品，或者如果他们希望整个交易保持在链上，则到智能合约中心市场（central sale aggregator smart contract）。通过这种方式，代币的所有者在其代币出售时保留其代币的保管权。

>在开始之前，我们需要确认您的帐户状态。
>
>如果您还没有，请执行上一教程中的步骤，以确保 Fungible Token 和 Non-Fungible Token 合约部署到账户 1 和 2 并拥有一些代币。您的帐户应如下所示：

>您可以运行 `CheckSetupScript.cdc` 脚本以确保您的帐户设置正确：

`CheckSetupScript.cdc`

```Cadence
import FungibleToken from 0x01
import NonFungibleToken from 0x02

// This script checks that the accounts are set up correctly for the marketplace tutorial.
//
// Account 0x01: Vault Balance = 40, NFT.id = 1
// Account 0x02: Vault Balance = 20, No NFTs
pub fun main() {
  // Get the accounts' public account objects
  let acct1 = getAccount(0x01)
  let acct2 = getAccount(0x02)

  // Get references to the account's receivers
  // by getting their public capability
  // and borrowing a reference from the capability
  let acct1ReceiverRef = acct1.getCapability<&FungibleToken.Vault{FungibleToken.Balance}>(/public/MainReceiver)
    .borrow()
    ?? panic("Could not borrow a reference to acc1 vault receiver")

  let acct2ReceiverRef = acct2.getCapability<&FungibleToken.Vault{FungibleToken.Balance}>(/public/MainReceiver)
    .borrow()
    ?? panic("Could not borrow a reference to acc2 vault receiver")

  // Log the Vault balance of both accounts and ensure they are
  // the correct numbers.
  // Account 0x01 should have 40.
  // Account 0x02 should have 20.
  log("Account 1 Balance")
  log(acct1ReceiverRef.balance)
  log("Account 2 Balance")
  log(acct2ReceiverRef.balance)

  // verify that the balances are correct
  if acct1ReceiverRef.balance != 40.0 || acct2ReceiverRef.balance != 40.0 {
      panic("Wrong balances!")
  }

  // Find the public Receiver capability for their Collections
  let acct1Capability = acct1.getCapability<&{NonFungibleToken.NFTReceiver}>(/public/NFTReceiver)
  let acct2Capability = acct2.getCapability<&{NonFungibleToken.NFTReceiver}>(/public/NFTReceiver)

  // borrow references from the capabilities
  let nft1Ref = acct1Capability.borrow()
        ?? panic("Could not borrow a reference to acc1 nft collection receiver")

  let nft2Ref = acct2Capability.borrow()
        ?? panic("Could not borrow a reference to acc2 nft collection receiver")

  // Print both collections as arrays of IDs
  log("Account 1 NFTs")
  log(nft1Ref.getIDs())

  log("Account 2 NFTs")
  log(nft2Ref.getIDs())

  // verify that the collections are correct
  if nft1Ref.getIDs()[0] != UInt64(1) || nft2Ref.getIDs().length != 0 {
      panic("Wrong Collections!")
  }
}
```

如果您的帐户设置正确，您应该会看到与此输出类似的内容。如果您连续遵循 Fungible Tokens 和 Non-Fungible Tokens 教程，它们将处于相同的状态：

```
"Account 1 Vault Balance"
40
"Account 2 Vault Balance"
20
"Account 1 NFTs"
[1]
"Account 2 NFTs"
[]
```

现在您的账户处于正确状态，我们可以建立一个市场，使账户之间的 NFT 售卖成为可能。





### **建立 NFT 市场**

---

每个想要出售 NFT 的用户都会在他们的帐户存储中存储一个 `SaleCollection` 资源的实例。

>切换到帐户 `0x03`。打开`Marketplace.cdc`
>
>打开 `Marketplace.cdc`，单击出现在编辑器右下角的 `Deploy` 按钮。 `Marketplace.cdc` 应包含以下合约定义：

`Marketplace.cdc`

```Cadence
import FungibleToken from 0x01
import NonFungibleToken from 0x02

// The Marketplace contract is a sample implementation of an NFT Marketplace on Flow.
//
// This contract allows users to put their NFTs up for sale. Other users
// can purchase these NFTs with fungible tokens.

pub contract Marketplace {

  // Event that is emitted when a new NFT is put up for sale
  pub event ForSale(id: UInt64, price: UFix64)

  // Event that is emitted when the price of an NFT changes
  pub event PriceChanged(id: UInt64, newPrice: UFix64)

  // Event that is emitted when a token is purchased
  pub event TokenPurchased(id: UInt64, price: UFix64)

  // Event that is emitted when a seller withdraws their NFT from the sale
  pub event SaleWithdrawn(id: UInt64)

  // Interface that users will publish for their Sale collection
  // that only exposes the methods that are supposed to be public
  //
  pub resource interface SalePublic {
    pub fun purchase(tokenID: UInt64, recipient: &AnyResource{NonFungibleToken.NFTReceiver}, buyTokens: @FungibleToken.Vault)
    pub fun idPrice(tokenID: UInt64): UFix64?
    pub fun getIDs(): [UInt64]
  }

  // SaleCollection
  //
  // NFT Collection object that allows a user to put their NFT up for sale
  // where others can send fungible tokens to purchase it
  //
  pub resource SaleCollection: SalePublic {

    // Dictionary of the NFTs that the user is putting up for sale
    pub var forSale: @{UInt64: NonFungibleToken.NFT}

    // Dictionary of the prices for each NFT by ID
    pub var prices: {UInt64: UFix64}

    // The fungible token vault of the owner of this sale.
    // When someone buys a token, this resource can deposit
    // tokens into their account.
    access(account) let ownerVault: Capability<&AnyResource{FungibleToken.Receiver}>

    init (vault: Capability<&AnyResource{FungibleToken.Receiver}>) {
        self.forSale <- {}
        self.ownerVault = vault
        self.prices = {}
    }

    // withdraw gives the owner the opportunity to remove a sale from the collection
    pub fun withdraw(tokenID: UInt64): @NonFungibleToken.NFT {
        // remove the price
        self.prices.remove(key: tokenID)
        // remove and return the token
        let token <- self.forSale.remove(key: tokenID) ?? panic("missing NFT")
        return <-token
    }

    // listForSale lists an NFT for sale in this collection
    pub fun listForSale(token: @NonFungibleToken.NFT, price: UFix64) {
        let id = token.id

        // store the price in the price array
        self.prices[id] = price

        // put the NFT into the the forSale dictionary
        let oldToken <- self.forSale[id] <- token
        destroy oldToken

        emit ForSale(id: id, price: price)
    }

    // changePrice changes the price of a token that is currently for sale
    pub fun changePrice(tokenID: UInt64, newPrice: UFix64) {
        self.prices[tokenID] = newPrice

        emit PriceChanged(id: tokenID, newPrice: newPrice)
    }

    // purchase lets a user send tokens to purchase an NFT that is for sale
    pub fun purchase(tokenID: UInt64, recipient: &AnyResource{NonFungibleToken.NFTReceiver}, buyTokens: @FungibleToken.Vault) {
        pre {
            self.forSale[tokenID] != nil && self.prices[tokenID] != nil:
                "No token matching this ID for sale!"
            buyTokens.balance >= (self.prices[tokenID] ?? 0.0):
                "Not enough tokens to by the NFT!"
        }

        // get the value out of the optional
        let price = self.prices[tokenID]!

        self.prices[tokenID] = nil

        let vaultRef = self.ownerVault.borrow()
            ?? panic("Could not borrow reference to owner token vault")
        
        // deposit the purchasing tokens into the owners vault
        vaultRef.deposit(from: <-buyTokens)

        // deposit the NFT into the buyers collection
        recipient.deposit(token: <-self.withdraw(tokenID: tokenID))

        emit TokenPurchased(id: tokenID, price: price)
    }

    // idPrice returns the price of a specific token in the sale
    pub fun idPrice(tokenID: UInt64): UFix64? {
        return self.prices[tokenID]
    }

    // getIDs returns an array of token IDs that are for sale
    pub fun getIDs(): [UInt64] {
        return self.forSale.keys
    }

    destroy() {
        destroy self.forSale
    }
  }

  // createCollection returns a new collection resource to the caller
  pub fun createSaleCollection(ownerVault: Capability<&AnyResource{FungibleToken.Receiver}>): @SaleCollection {
    return <- create SaleCollection(vault: ownerVault)
  }
}
```

该市场合约的资源功能与非同质化代币中解释的 NFT 集合`Collection`类似，但有一些差异和补充：

+ 该市场合约具有添加和删除 NFT 的方法，但这些功能还涉及设置和删除价格。当用户想要出售他们的 NFT 时，他们会使用 `listForSale` 函数将其存入收藏中。然后，另一个用户可以调用购买`purchase`方法，发送包含他们用于购买的货币的 `Vault`。买家还包括对他们的 NFT 收藏`Collection`的引用，以便购买的代币可以在购买时立即存入他们的收藏。

+ 这个市场合约存储了一个能力:

  `pub let ownerVault: Capability<&AnyResource{FungibleToken.Receiver}>`。售卖的所有者在销售中将功能定位符保存到他们的 `Fungible Token Receiver`。这使得售卖的资源能够在购买时立即将用于购买 NFT 的货币存入所有者钱包`Vault`。

+ 市场合同包括事件函数。 Cadence 支持在合约中定义事件，当重要动作发生时可以调用这些事件函数。

```Cadence
pub event ForSale(id: UInt64, price: UFix64)
pub event PriceChanged(id: UInt64, newPrice: UFix64)
pub event TokenPurchased(id: UInt64, price: UFix64)
pub event SaleWithdrawn(id: UInt64)
```

事件是通过访问级别(`pub`)、事件关键字(`event`)以及事件的名称和参数来声明的，就像函数声明一样。事件不能修改状态；它们表示智能合约中何时发生重要操作。事件是用`emit` 关键字发出的，然后是调用事件，就好像它是一个函数调用一样。外部应用程序可以监视区块链以在发出某些事件时采取行动。

此时，我们应该在两个帐户的存储中都有一个同质化代币钱包`Vault`和一个 NFT 集合`Collection`。帐户 `0x01` 的集合中应该有 NFT。

您可以按照以下步骤创建一个 `SaleCollection` 并列出账户 `0x01` 的代币进行售卖：

>打开`Transaction1.cdc`
>
>选择账户 `0x01` 作为唯一的签名者，然后点击发送`Send`按钮提交交易。

`Transaction1.cdc`

```Cadence
import FungibleToken from 0x01
import NonFungibleToken from 0x02
import Marketplace from 0x03

// This transaction creates a new Sale Collection object,
// lists an NFT for sale, puts it in account storage,
// and creates a public capability to the sale so that others can buy the token.
transaction {

  prepare(acct: AuthAccount) {

    // Borrow a reference to the stored Vault
    let receiver = acct.getCapability<&{FungibleToken.Receiver}>(/public/MainReceiver)

    // Create a new Sale object,
    // initializing it with the reference to the owner's vault
    let sale <- Marketplace.createSaleCollection(ownerVault: receiver)

    // borrow a reference to the NFTCollection in storage
    let collectionRef = acct.borrow<&NonFungibleToken.Collection>(from: /storage/NFTCollection)
            ?? panic("Could not borrow a reference to the owner's nft collection")

    // Withdraw the NFT from the collection that you want to sell
    // and move it into the transaction's context
    let token <- collectionRef.withdraw(withdrawID: 1)

    // List the token for sale by moving it into the sale object
    sale.listForSale(token: <-token, price: 10.0)

    // Store the sale object in the account storage
    acct.save<@Marketplace.SaleCollection>(<-sale, to: /storage/NFTSale)

    // Create a public capability to the sale so that others can call its methods
    acct.link<&Marketplace.SaleCollection{Marketplace.SalePublic}>(/public/NFTSale, target: /storage/NFTSale)

    log("Sale Created for account 1. Selling NFT 1 for 10 tokens")
  }
}
```

这份交易进行了如下步骤：

1. 获取所有者 `Vault` 的功能定位符。
2. 创建 `SaleCollection`，用于存储他们的 `Vault` 引用。
3. 从他们的钱包中提取所有者的代币。
4. 列出要出售的代币并设定其价格。
5. 存储在他们的帐户中，并公开一个功能定位符，允许其他人购买任何 NFT 进行售卖。

让我们运行一个脚本来确保正确创建了售卖。

>打开`Script2.cdc`
>
>点击 `Execute` 按钮，打印账户 `0x01` 所拥有的 NFT 的 ID 和价格sale.s

`Script2.cdc`

```Cadnece
import FungibleToken from 0x01
import NonFungibleToken from 0x02
import Marketplace from 0x03

// This script prints the NFTs that account 0x01 has for sale.
pub fun main() {
  // Get the public account object for account 0x01
  let account1 = getAccount(0x01)

  // Find the public Sale reference to their Collection
  let acct1saleRef = account1.getCapability<&AnyResource{Marketplace.SalePublic}>(/public/NFTSale)
        .borrow()
        ?? panic("Could not borrow a reference to the sale")

  // Los the NFTs that are for sale
  log("Account 1 NFTs for sale")
  log(acct1saleRef.getIDs())
  log("Price")
  log(acct1saleRef.idPrice(tokenID: 1))
}
```

此脚本执行完毕后并打印如下内容：

```
"Account 1 NFTs for sale"
[1]
"Price"
10
```





### **购买 NFT**

---

买家现在可以使用`Transaction2.cdc`交易来购买卖家提供的NFT。

>打开`Transaction2.cdc`文件
>
>选择帐户 `0x02` 作为唯一的签名者，然后单击`Send`按钮。

`Transaction2.cdc`

```Cadence
import FungibleToken from 0x01
import NonFungibleToken from 0x02
import Marketplace from 0x03

// This transaction uses the signer's Vault tokens to purchase an NFT
// from the Sale collection of account 0x01.
transaction {

  // reference to the buyer's NFT collection where they
  // will store the bought NFT
  let collectionRef: &AnyResource{NonFungibleToken.NFTReceiver}

  // Vault that will hold the tokens that will be used to
  // but the NFT
  let temporaryVault: @FungibleToken.Vault

  prepare(acct: AuthAccount) {

    // get the references to the buyer's fungible token Vault
    // and NFT Collection Receiver
    self.collectionRef = acct.borrow<&AnyResource{NonFungibleToken.NFTReceiver}>(from: /storage/NFTCollection)
        ?? panic("Could not borrow reference to the signer's nft collection")

    let vaultRef = acct.borrow<&FungibleToken.Vault>(from: /storage/MainVault)
        ?? panic("Could not borrow reference to the signer's vault")

    // withdraw tokens from the buyers Vault
    self.temporaryVault <- vaultRef.withdraw(amount: 10.0)
  }

  execute {
    // get the read-only account storage of the seller
    let seller = getAccount(0x01)

    // get the reference to the seller's sale
    let saleRef = seller.getCapability<&AnyResource{Marketplace.SalePublic}>(/public/NFTSale)
        .borrow()
        ?? panic("could not borrow reference to the seller's sale")

    // purchase the NFT the the seller is selling, giving them the reference
    // to your NFT collection and giving them the tokens to buy it
    saleRef.purchase(tokenID: 1,
        recipient: self.collectionRef,
        buyTokens: <-self.temporaryVault)

    log("Token 1 has been bought by account 2!")
  }
}

```

这笔交易：

1. 获取账户 `0x01` 的公有账户对象。
2. 获取对买家资源的引用。
3. 取出买方将用于购买 NFT 的代币。
4. 获取对卖家公开售卖的引用。
5. 调用`purchase`购买函数，传入代币和`Collection`收藏的引用。然后执行购买将买入的 NFT 直接存入买家的`Collection`收藏中。



### **验证 NFT交易是否正确执行**

---

您现在可以运行脚本来验证是否正确购买了 NFT，因为：

+ 账户 `0x01` 有 50 个代币，并且在他们的收藏和账户中没有任何 NFT 出售.
+ 账户 `0x02` 有 10 个代币和一个 id=1 的 NFT

要运行验证 NFT 购买是否正确的脚本，请执行以下步骤：

>打开“Script3.cdc”文件。
>
>点击`Execute`按钮`Script3.cdc`应该包含以下代码：

`Script3.cdc`

```Cadence
import FungibleToken from 0x01
import NonFungibleToken from 0x02
import Marketplace from 0x03

// This script checks that the Vault balances and NFT collections are correct
// for both accounts.
//
// Account 1: Vault balance = 50, No NFTs
// Account 2: Vault balance = 10, NFT ID=1
pub fun main() {
  // Get the accounts' public account objects
  let acct1 = getAccount(0x01)
  let acct2 = getAccount(0x02)

  // Get references to the account's receivers
  // by getting their public capability
  // and borrowing a reference from the capability
  let acct1ReceiverRef = acct1.getCapability<&FungibleToken.Vault{FungibleToken.Balance}>(/public/MainReceiver)
        .borrow()
        ?? panic("Could not borrow reference to acct1 vault")

  let acct2ReceiverRef = acct2.getCapability<&FungibleToken.Vault{FungibleToken.Balance}>(/public/MainReceiver)
        .borrow()
        ?? panic("Could not borrow reference to acct2 vault")

  // Log the Vault balance of both accounts and ensure they are
  // the correct numbers.
  // Account 0x01 should have 50.
  // Account 0x02 should have 10.
  log("Account 1 Balance")
  log(acct1ReceiverRef.balance)
  log("Account 2 Balance")
  log(acct2ReceiverRef.balance)

  // verify that the balances are correct
  if acct1ReceiverRef.balance != 50.0
     || acct2ReceiverRef.balance != 10.0
  {
      panic("Wrong balances!")
  }

  // Find the public Receiver capability for their Collections
  let acct1Capability = acct1.getCapability<&{NonFungibleToken.NFTReceiver}>(/public/NFTReceiver)
  let acct2Capability = acct2.getCapability<&{NonFungibleToken.NFTReceiver}>(/public/NFTReceiver)

  // borrow references from the capabilities
  let nft1Ref = acct1Capability.borrow()
    ?? panic("Could not borrow reference to acct1 nft collection")

  let nft2Ref = acct2Capability.borrow()
    ?? panic("Could not borrow reference to acct2 nft collection")

  // Print both collections as arrays of IDs
  log("Account 1 NFTs")
  log(nft1Ref.getIDs())

  log("Account 2 NFTs")
  log(nft2Ref.getIDs())

  // verify that the collections are correct
  if nft2Ref.getIDs()[0] != 1 as UInt64 || nft1Ref.getIDs().length != 0 {
      panic("Wrong Collections!")
  }

  // Get the public sale reference for Account 0x01
  let acct1SaleRef = acct1.getCapability<&AnyResource{Marketplace.SalePublic}>(/public/NFTSale)
        .borrow()
        ?? panic("Could not borrow a reference to the sale")

  // Print the NFTs that account 0x01 has for sale
  log("Account 1 NFTs for sale")
  log(acct1SaleRef.getIDs())
  if acct1SaleRef.getIDs().length != 0 { panic("Sale should be empty!") }
}
```

如果你做的一切都正确，交易应该会成功，它应该打印类似这样的东西：

```
"Account 1 Vault Balance"
50
"Account 2 Vault Balance"
10
"Account 1 NFTs"
[]
"Account 2 NFTs"
[1]
"Account 1 NFTs for Sale"
[]
```

恭喜您，您已经成功地在 Cadence 中实现了一个简单的市场，并使用它来允许一个账户从另一个账户购买 NFT！



### **扩展市场**

---

用户可以使用这些资源和交易在他们的帐户中进行售卖。对于中心市场的支持可以使用户相对便利地去操作和在原有的基础上建立服务。如果我们想在链上建立一个中央市场，我们可以使用如下所示的合约：

`CentralMarketplace.cdc`

```Cadence
// Marketplace would be the central contract where people can post their sale
// references so that anyone can access them
pub contract Marketplace {
    // Data structure to store active sales
    pub var tokensForSale: [Capability<&SaleCollection>]

    // listSaleCollection lists a users sale reference in the array
    // and returns the index of the sale so that users can know
    // how to remove it from the marketplace
    pub fun listSaleCollection(collection: Capability<&SaleCollection>): Int {
        self.tokensForSale.append(collection)
        return (self.tokensForSale.length - 1)
    }

    // removeSaleCollection removes a user's sale from the array
    // of sale references
    pub fun removeSaleCollection(index: Int) {
        self.tokensForSale.remove(at: index)
    }

}
```

该合约并不意味着是一个工作或生产环境下的合约，但它可以通过以下方式扩展为一个完整的中央市场：

+ 卖方在本合同中为其 `SaleCollection` 列出了一项功能定位符。
+ 买家可以调用其他功能函数以获取有关所有不同售卖的信息并进行购买。

链下应用程序中的中央市场更容易实现，因为：

+ 该应用程序可以托管市场，用户只需登录该应用程序并为该应用程序提供其帐户地址即可。
+ 该应用程序可以读取用户的公有存储并找到他们的售卖引用。
+ 通过售卖引用，该应用程序可以获得有关如何在其网站上显示销售情况的所有信息。
+ 任何买家都可以在应用程序中发现售卖信息并使用他们的帐户登录，这使应用程序可以访问他们的公开引用。
+ 当买家想要购买特定的 NFT 时，应用程序会自动生成正确的交易以从卖家那里购买 NFT。



### **为任何通用 NFT 创建市场**

---

前面的例子展示了如何为特定类别的 NFT 创建一个简单的市场。但是，用户希望有一个市场，无论其类型如何，他们都可以在其中买卖任何他们想要的 NFT。对此功能的支持正在开发中，很快就会分享。



### **Flow上的复合资源**

---

现在您已经了解了可组合智能合约和市场如何在 Flow 上工作，您已准备好使用复合资源！







## **7.复合资源**

---

在本教程中，我们将通过创建、部署和移动可组合 NFT 来学习资源如何拥有其他资源。

---

>在 Flow Playground 打开本教程的入门代码：
>
>https://play.onflow.org/fb934880-ba3a-4d0d-8350-7bea017e6138
>
>本教程将要求您采取各种措施与此代码进行交互。

>需要您采取行动的说明总是包含在这样的标注框中。
>
>这些突出显示的操作是让代码运行所需的全部操作，但阅读其余部分对于理解语言的设计是必要的。

拥有其他资源的资源是区块链和智能合约世界中的一个强大功能。为了展示此功能在 Flow 上的工作原理，本教程将通过可组合 NFT 引导您完成以下步骤：

1. 将 `Kitty` 和 `KittyHat` 定义部署到帐户 `0x01`。
2. 创建一个 `Kitty` 和两个 `KittyHats` 并将它们存储在您的帐户中。
3. 移动小猫和帽子，看看复合的 NFT 如何在 Flow 上发挥作用。

在继续本教程之前，我们建议您按照入门和 Hello, World! 中的说明进行操作。以便了解开发者平台和 Cadence。

如需更多支持，请参阅 [开发者平台手册](https://docs.onflow.org/intro/playground-manual/)。



### **拥有资源的资源**

---

在[非同质化代币](https://docs.onflow.org/cadence/tutorial/04-non-fungible-tokens)谈到的 NFT 集合是拥有其他资源的资源的例子。我们拥有一个资源，一个NFT集合，它拥有存储在其中的 NFT 资源的所有权。所有者和任何拥有引用的人都可以移动这些资源，但当它们在集合中时它们仍然属于集合，并且集合中定义的代码对资源具有最终控制权。

当集合被移动或销毁时，其中的所有 NFT 都会随之移动或销毁。

如果集合的所有者将整个收藏资源转移到另一个用户的帐户，所有代币都将移动到其他用户的帐户中。代币不会留在原始所有者的帐户中。这就像把你的钱包而不是一美元的钞票交给别人。这不是一个常见的动作，但肯定是可能的。

无法为存储在其他资源中的资源创建引用。拥有资源可以控制它，因此控制外部调用对存储资源的访问类型。



### **拥有资源的资源：一个例子**

---

NFT 集合是资源如何拥有其他资源的一个简单示例，但可以制作一个更强大的版本。

CryptoKitties（以及以太坊区块链上的应用程序）的一个重要特性是任何开发人员都可以围绕现有应用程序创造新体验。即使原始合同不包括对 CryptoKitty 配件（如帽子）的具体支持，独立开发人员仍然能够制作原始合同中 Kitties 可以使用的帽子。

以下是我们如何在 Cadence 中复制此功能的基本示例：

>打开帐户 `0x01` 选项卡，其中包含名为 `KittyVerse.cdc` 的合约。将代码部署到账户 `0x01`

`KittyVerse.cdc`

```Cadence
// The KittyVerse contract defines two types of NFTs.
// One is a KittyHat, which represents a special hat, and
// the second is the Kitty resource, which can own Kitty Hats.
//
// You can put the hats on the cats and then call a hat function
// that tips the hat and prints a fun message.
//
// This is a simple example of how Cadence supports
// extensibility for smart contracts, but the language will soon
// support even more powerful versions of this.

pub contract KittyVerse {

    // KittyHat is a special resource type that represents a hat
    pub resource KittyHat {
        pub let id: Int
        pub let name: String

        init(id: Int, name: String) {
            self.id = id
            self.name = name
        }

        // An example of a function someone might put in their hat resource
        pub fun tipHat(): String {
            if self.name == "Cowboy Hat" {
                return "Howdy Y'all"
            } else if self.name == "Top Hat" {
                return "Greetings, fellow aristocats!"
            }

            return "Hello"
        }
    }

    // Create a new hat
    pub fun createHat(id: Int, name: String): @KittyHat {
        return <-create KittyHat(id: id, name: name)
    }

    pub resource Kitty {

        pub let id: Int

        // place where the Kitty hats are stored
        pub let items: @{String: KittyHat}

        init(newID: Int) {
            self.id = newID
            self.items <- {}
        }

        destroy() {
            destroy self.items
        }
    }

    pub fun createKitty(): @Kitty {
        return <-create Kitty(newID: 1)
    }
}
```

这些定义显示了 Kitty 资源如何拥有帽子。

帽子存储在 Kitty 资源中的一个变量中。

```
// place where the Kitty hats are stored
pub let items: <-{String: KittyHat}
```

Kitty 主人可以摘下 Kitty 的帽子并单独转让。或者主人可以转让一只拥有帽子的小猫，帽子会和小猫一起去。

这是一个创建 `Kitty` 和 `KittyHat` 的交易，将帽子存储在 `Kitty` 中，然后将其存储在您的帐户存储中。

>打开`Transaction1.cdc`。
>
>选择账户 `0x01` 作为唯一的签名者，单击“发送”按钮发送交易。
>
>`Transaction1.cdc` 应包含以下代码：

`Transaction1.cdc`

```Cadence
import KittyVerse from 0x01

// This transaction creates a new kitty, creates two new hats and
// puts the hats on the cat. Then it stores the kitty in account storage.
transaction {
  prepare(acct: AuthAccount) {

    // Create the Kitty object
    let kitty <- KittyVerse.createKitty()

    // Create the KittyHat objects
    let hat1 <- KittyVerse.createHat(id: 1, name: "Cowboy Hat")
    let hat2 <- KittyVerse.createHat(id: 2, name: "Top Hat")

    // Put the hat on the cat!
    let oldCowboyHat <- kitty.items["Cowboy Hat"] <- hat1
    destroy oldCowboyHat
    let oldTopHat <- kitty.items["Top Hat"] <- hat2
    destroy oldTopHat

    log("The cat has the hats")

    // Store the Kitty in storage
    acct.save<@KittyVerse.Kitty>(<-kitty, to: /storage/Kitty)
  }
}
```

您应该会看到如下所示的输出：

```
> "The Cat has the Hats"
```

现在我们可以运行一个事务来移动小猫和它的帽子，从小猫上取下牛仔帽，然后让小猫摘掉它的帽子。

> 打开`Transaction2.cdc`。
>
> 选择账户 `0x01` 作为唯一的签名者，单击“发送”按钮发送交易。
>
> `Transaction2.cdc` 应包含以下代码：

`Transaction2.cdc`

```Cadence
import KittyVerse from 0x01

// This transaction moves a kitty out of storage,
// takes the cowboy hat off of the kitty,
// calls its tip hat function, and then moves it back into storage.
transaction {
  prepare(acct: AuthAccount) {

    // Move the Kitty out of storage, which also moves its hat along with it
    let kitty <- acct.load<@KittyVerse.Kitty>(from: /storage/Kitty)!

    // Take the cowboy hat off the Kitty
    let cowboyHat <- kitty.items.remove(key: "Cowboy Hat")!

    // Tip the cowboy hat
    log(cowboyHat.tipHat())
    destroy cowboyHat

    // Tip the top hat that is on the Kitty
    log(kitty.items["Top Hat"]?.tipHat())

    // Move the Kitty to storage, which
    // also moves its hat along with it.
    acct.save<@KittyVerse.Kitty>(<-kitty, to: /storage/Kitty)
  }
}
```

您应该看到类似这样的输出：

```Cadence
> "Howdy Y'all"
> "Greetings, fellow aristocats!"
```

每当 Kitty 被移动时，它的帽子也隐含地随之移动。这是因为帽子是小猫所有的。





### **未来是喵喵的！可扩展性时代即将到来！**

---

以上是复合资源的一个简单示例。我们必须明确地说明了Kitty猫戴着它的帽子，但是在不远的未来，Cadence将会支持更加强大的资源可扩展性，开发人员可以声明单独资源可以拥有的类型，即使拥有的资源从一开始就没有指定所有权的可能性。这是一个非常复杂的问题，需要以安全的方式解决，Flow 社区正在非常努力地为此设计解决方案，但它即将到来。

练习您在Flow开发者平台学习到的知识！



## **8.投票合约**

---

在本教程中，我们将部署一个合约，允许用户对投票管理员控制的多个提案进行投票。

---

>在Flow开发者社区中打开本教程的入门代码 ：
>
>https://play.onflow.org/a4db333e-0c7d-422f-b7bc-00fc3f341fd8?type=account&id=0
>
>本教程将要求您采取各种操作来与此代码进行交互。

>需要您采取行动的说明总是包含在这样的标注框中。
>
>这些突出显示的操作是让代码运行所需的全部操作，但阅读其余部分对于理解语言的设计是必要的。

随着区块链技术和智能合约的出现，尝试创建去中心化的投票机制，允许大量用户完全在链上投票已经成为一种趋势。本教程将提供一个简单的示例，说明如何使用面向资源的编程模型来实现这一点。

我们将带您完成这些步骤，以熟悉投票合同。

1. 将合约部署到账户 1。
2. 创建供用户投票的提案。
3. 使用具有多个签名者的交易直接将 `Ballot` 资源转移到另一个帐户。
4. 在中央投票合约中记录并投票。
5. 读取投票结果。

在继续本教程之前，我们强烈建议您按照[入门](https://docs.onflow.org/cadence/tutorial/01-first-steps/)和 [Hello World ](https://docs.onflow.org/cadence/tutorial/02-hello-world/)中的说明学习如何使用开发者平台工具并学习 Cadence 的基础知识。





### **Cadence 中的投票合约**

---

在本合约中，选票被表示为一种资源。管理员可以给其他账户投票，然后这些账户标记他们投票给哪些提案，并将投票提交给中央智能合约以记录他们的投票。对于此应用程序使用资源类型是符合逻辑的，因为如果用户想要委托他们的投票，他们可以将该选票发送到另一个帐户。





### 部署合约

---

> 打开具有 `ApprovalVoting `合约的账户 `0x01`。
>
> 单击“部署”按钮将其部署到帐户 `0x01`。

`ApprovalVoting.cdc`

```Cadence
/*
*
*   In this example, we want to create a simple approval voting contract
*   where a polling place issues ballots to addresses.
*
*   The run a vote, the Admin deploys the smart contract,
*   then initializes the proposals
*   using the initialize_proposals.cdc transaction.
*   The array of proposals cannot be modified after it has been initialized.
*
*   Then they will give ballots to users by
*   using the issue_ballot.cdc transaction.
*
*   Every user with a ballot is allowed to approve any number of proposals.
*   A user can choose their votes and cast them
*   with the cast_vote.cdc transaction.
*
*/

pub contract ApprovalVoting {

    //list of proposals to be approved
    pub var proposals: [String]

    // number of votes per proposal
    pub let votes: {Int: Int}

    // This is the resource that is issued to users.
    // When a user gets a Ballot object, they call the `vote` function
    // to include their votes, and then cast it in the smart contract
    // using the `cast` function to have their vote included in the polling
    pub resource Ballot {

        // array of all the proposals
        pub let proposals: [String]

        // corresponds to an array index in proposals after a vote
        pub var choices: {Int: Bool}

        init() {
            self.proposals = ApprovalVoting.proposals
            self.choices = {}

            // Set each choice to false
            var i = 0
            while i < self.proposals.length {
                self.choices[i] = false
                i = i + 1
            }
        }

        // modifies the ballot
        // to indicate which proposals it is voting for
        pub fun vote(proposal: Int) {
            pre {
                self.proposals[proposal] != nil: "Cannot vote for a proposal that doesn't exist"
            }
            self.choices[proposal] = true
        }
    }

    // Resource that the Administrator of the vote controls to
    // initialize the proposals and to pass out ballot resources to voters
    pub resource Administrator {

        // function to initialize all the proposals for the voting
        pub fun initializeProposals(_ proposals: [String]) {
            pre {
                ApprovalVoting.proposals.length == 0: "Proposals can only be initialized once"
                proposals.length > 0: "Cannot initialize with no proposals"
            }
            ApprovalVoting.proposals = proposals

            // Set each tally of votes to zero
            var i = 0
            while i < proposals.length {
                ApprovalVoting.votes[i] = 0
                i = i + 1
            }
        }

        // The admin calls this function to create a new Ballo
        // that can be transferred to another user
        pub fun issueBallot(): @Ballot {
            return <-create Ballot()
        }
    }

    // A user moves their ballot to this function in the contract where
    // its votes are tallied and the ballot is destroyed
    pub fun cast(ballot: @Ballot) {
        var index = 0
        // look through the ballot
        while index < self.proposals.length {
            if ballot.choices[index]! {
                // tally the vote if it is approved
                self.votes[index] = self.votes[index]! + 1
            }
            index = index + 1;
        }
        // Destroy the ballot because it has been tallied
        destroy ballot
    }

    // initializes the contract by setting the proposals and votes to empty
    // and creating a new Admin resource to put in storage
    init() {
        self.proposals = []
        self.votes = {}

        self.account.save<@Administrator>(<-create Administrator(), to: /storage/VotingAdmin)
    }
}

```

这份合约实现了一个简单的投票机制，在这里一个管理员可以使用`initializeProposals`函数来初始化一个提案数组去投票。

```Cadence
// function to initialize all the proposals for the voting
pub fun initializeProposals(_ proposals: [String]) {
    pre {
        ApprovalVoting.proposals.length == 0: "Proposals can only be initialized once"
        proposals.length > 0: "Cannot initialize with no proposals"
    }
    ApprovalVoting.proposals = proposals

    // Set each tally of votes to zero
    var i = 0
    while i < proposals.length {
        ApprovalVoting.votes[i] = 0
        i = i + 1
    }
}
```

然后他们可以把`Ballot`资源转让给其他账户。其他账户也可以调用`vote`函数来记录他们的`Ballot`资源。

```
pub fun vote(proposal: Int) {
    pre {
        self.proposals[proposal] != nil: "Cannot vote for a proposal that doesn't exist"
    }
    self.choices[proposal] = true
}
```

在一个用户投票完后，他们通过调用`cast`函数来提交投票结果到中心智能合约上，这记录了已经投过的`Ballot`和销毁使用过的`Ballot`。

```Cadence
// A user moves their ballot to this function in the contract where
// its votes are tallied and the ballot is destroyed
pub fun cast(ballot: @Ballot) {
    var index = 0
    // look through the ballot
    while index < self.proposals.length {
        if ballot.choices[index]! {
            // tally the vote if it is approved
            self.votes[index] = self.votes[index]! + 1
        }
        index = index + 1;
    }
    // Destroy the ballot because it has been tallied
    destroy ballot
}
```

当投票时间结束，如果提案接收到正确的投票数，管理员可以读取每个提案的结果。



### **运行投票**

---

执行此投票合约中的常见操作只需要三种类型的交易。

1. 初始化提案
2. 将`Ballot`发送给选民
3. 投票

我们为您提供的每一步都有一个交易。

>您应该已将 ApprovalVoting 部署到帐户 `0x01`。
>
>应该有 `Transaction1.cdc` 的打开事务 1，将账户 `0x01` 选为唯一签名者提交交易。

`Transaction1.cdc`

```Cadence
import ApprovalVoting from 0x01

// This transaction allows the administrator of the Voting contract
// to create new proposals for voting and save them to the smart contract

transaction {
    prepare(admin: AuthAccount) {

        // borrow a reference to the admin Resource
        let adminRef = admin.borrow<&ApprovalVoting.Administrator>(from: /storage/VotingAdmin)!

        // Call the initializeProposals function
        // to create the proposals array as an array of strings
        adminRef.initializeProposals(
            ["Longer Shot Clock", "Trampolines instead of hardwood floors"]
        )

        log("Proposals Initialized!")
    }

    post {
        ApprovalVoting.proposals.length == 2
    }

}
```

该交易允许投票合约的管理员创建新的投票提案并将其保存到智能合约中。他们通过在管理员`Administrator`资源上调用 `initializeProposals` 函数来做到这一点，给它两个新的提案进行投票。我们使用 `post` 块来确保创建了两个提案，就像我们希望的那样。

接下来，管理员需要将选票分发给选民。这次没有简单的存款`deposit`功能，他们可以将选票`Ballot`发送到另一个帐户，那么他们将如何做？这是多交易签名者可以派上用场的地方！





### **选择多个帐户作为签名者**

---

一个交易可以访问每个签署它的账户的私人账户对象，所以如果管理员和投票者都签署了一个交易，管理员可以直接将一个选票`Ballot`资源对象移动到另一个账户的存储中。

在 Flow开发者平台中，您可以选择多个账户来签署交易，以便能够访问两个账户的私有账户对象。

要选择多个签名者，您首先需要在交易的准备`prepare`块中包含两个参数：

`prepare(acct1: AuthAccount, acct2: AuthAccount)`

如果所选签名者的数量与准备块的参数数量不同，开发者平台会给你一个错误。开发者平台还会按照您选择的顺序将您选择作为签名者的帐户映射到参数。您选择的第一个帐户将是第一个参数，您选择的第二个帐户是第二个参数。

>打开 `Transaction2.cdc` 事务 2。
>
>首先选择帐户 `0x01` 作为签名者，然后再选择帐户 `0x02`。
>
>点击发送`Send`按钮提交交易。

`Transaction2.cdc`

```Cadence

import ApprovalVoting from 0x01

// This transaction allows the administrator of the Voting contract
// to create a new ballot and store it in a voter's account
// The voter and the administrator have to both sign the transaction
// so it can access their storage

transaction {
    prepare(admin: AuthAccount, voter: AuthAccount) {

        // borrow a reference to the admin Resource
        let adminRef = admin.borrow<&ApprovalVoting.Administrator>(from: /storage/VotingAdmin)!

        // create a new Ballot by calling the issueBallot
        // function of the admin Reference
        let ballot <- adminRef.issueBallot()

        // store that ballot in the voter's account storage
        voter.save<@ApprovalVoting.Ballot>(<-ballot, to: /storage/Ballot)

        log("Ballot transferred to voter")
    }
}

```

该交易有两个签名者作为准备`prepare`参数，因此它能够访问他们的两个私有 `AuthAccount` 对象，从而访问他们的私有帐户存储。因此，我们可以通过使用 admin 的 `issueBallot` 函数创建它来直接执行 `Ballot` ，然后使用 `save` 函数将其直接存储在选民的存储中。

帐户 `0x02` 现在应该在其帐户存储中有一个 `Ballot` 资源对象。您可以通过打开帐户 2 选项卡并查看资源框中显示的选票`Ballot`资源来确认这一点。





### **投票**

---

现在帐户 `0x02` 的存储中有选票`Ballot`，他们可以投票。为此，他们将对其存储的资源调用投票`vote`方法，然后通过将其传递给主智能合约中的 `cast` 函数来投出该选票`Ballot`。

>打开`Transaction3.cdc` 事务 3。
>
>选择账户 `0x02` 作为唯一的交易签名者。
>
>点击发送`Send`按钮提交交易。

`Transaction3.cdc`

```Cadence
import ApprovalVoting from 0x01

// This transaction allows a voter to select the votes they would like to make
// and cast that vote by using the castVote function
// of the ApprovalVoting smart contract

transaction {
    prepare(voter: AuthAccount) {

        // take the voter's ballot our of storage
        let ballot <- voter.load<@ApprovalVoting.Ballot>(from: /storage/Ballot)!

        // Vote on the proposal
        ballot.vote(proposal: 1)

        // Cast the vote by submitting it to the smart contract
        ApprovalVoting.cast(ballot: <-ballot)

        log("Vote cast and tallied")
    }
}
```

在此交易中，用户对其中一个提案进行投票，然后将他们的选票移回计票的智能合约。





### **查看投票结果**

---

在任何时候，任何人都可以通过直接读取合约的字段来读取当前的投票数。您可以使用脚本来执行此操作，因为它不需要修改存储内容。

>打开应包含以下代码的脚本 1。
>
>单击执行`execute`按钮以运行脚本。

`Script1.cdc`

```Cadence
import ApprovalVoting from 0x01

// This script allows anyone to read the tallied votes for each proposal
//

pub fun main() {

    // Access the public fields of the contract to log
    // the proposal names and vote counts

    log("Number of Votes for Proposal 1:")
    log(ApprovalVoting.proposals[0])
    log(ApprovalVoting.votes[0])

    log("Number of Votes for Proposal 2:")
    log(ApprovalVoting.proposals[1])
    log(ApprovalVoting.votes[1])

}
```

你应该看到类似这样的打印：

```Cadence
"Number of Votes for Proposal 1:"
"Longer Shot Clock"
0
"Number of Votes for Proposal 2:"
"Trampolines instead of hardwood floors"
1
```

这表明提案 1 投了一票，提案 2 没有投一票。





### **其他投票可能性**

---

这份合约是在 Cadence 中投票的一个非常简单的例子。它显然不能用于现实世界的投票情况，但希望您能看到可以添加什么样的功能来确保实用性和安全性。

