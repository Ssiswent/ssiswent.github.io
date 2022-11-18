---
title: Flutter面试
excerpt: 包含了 Dart、Flutter、iOS 的部分知识点
index_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221101154309.png
banner_img: https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221101154334.png
tags: [Flutter]
category_bar: true
categories: [Flutter]
---

# 大纲

![大纲](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/xmind_flutter.png)

## Dart 部分

### 1. 级联操作符

{% note secondary %}
Dart 当中的 `..` 意思是 **级联操作符**，为了方便配置而使用。
`..` 和 `.` 不同的是 调用 `..` 后返回的相当于是 this，而 `.` 返回的则是该方法返回的值。

```dart
person..name = '李四'
      ..age = 30
      ..printInfo();
```

{% endnote %}

### 2. `dynamic` , `var` , `object` 的区别

{% note secondary %}
`var` 如果有初始值，类型被锁定，而 `dynamic` 和 `object` 都是动态任意类型
`dynamic` 编译阶段不检查类型，调用不存在的方法和变量不会报错
`object` 编译阶段检查类型，调用不存在的方法和变量会报错
{% endnote %}

### 3. `const` 和 `final` 的区别

{% note secondary %}
**相同点：**

1. 类型声明可以省略

```dart
final String a = 'Ssiswent';
final a = 'Ssiswent';

const String a = 'Ssiswent';
const a = 'Ssiswent';
```

2. 声明时必须要赋值，初始化后不能再赋值
3. 不能和 `var` 同时使用

**不同点：**

1. `const` 需要确定的值，初始值只能是编译时确定的值，比如当前时间

```dart
final dt = DateTime.now(); ✅

const dt = const DateTime.now(); ❎
```

2. `const` 修饰的 List 集合任意索引不可修改，`final` 修饰的可以修改

```dart
final List ls = [11, 22, 33];
ls[1] = 44;

// 编译时不报错，运行时提示出错
const List ls = [11, 22, 33];
ls[1] = 44;
```

3. 相同的值，`final` 会在内存中重复创建，`const` 会引用同一份值
   {% endnote %}

### 4. Dart 的作用域

{% note secondary %}
Dart 没有 `public` `private` 等关键字，默认就是公开的，私有变量使用下划线 `_` 开头。
{% endnote %}

### 5. Dart 的线程模型以及运行方式

{% note secondary %}
Dart 是单线程模型，Dart 在单线程中是以消息循环机制来运行的，包含两个任务队列，一个是“微任务队列” microtask queue，另一个叫做“事件队列” event queue。
![运行流程](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221108171607.png)
当 Flutter 应用启动后，消息循环机制便启动了。首先会按照先进先出的顺序逐个执行微任务队列中的任务，当所有微任务队列执行完后便开始执行事件队列中的任务，事件任务执行完毕后再去执行微任务，如此循环往复。
{% endnote %}

### 6. Dart 是如何实现多任务并行的

{% note secondary %}
Flutter 的多线程主要依赖 Dart 的并发编程、异步和事件驱动机制。
![isolate交互](https://cdn.jsdelivr.net/gh/Ssiswent/myBlogResource/images/20221108134845.png)
在 Dart 中，一个 Isolate 对象其实就是一个 isolate 执行环境的引用，一般来说我们都是通过当前的 isolate 去控制其他的 isolate 完成彼此之间的交互，而当我们想要创建一个新的 Isolate 可以使用 `Isolate.spawn` 方法获取返回的一个新的 isolate 对象，两个 isolate 之间使用 SendPort 相互发送消息，而 isolate 中也存在了一个与之对应的 ReceivePort 接受消息用来处理，但是我们需要注意的是，ReceivePort 和 SendPort 在每个 isolate 都有一对，只有同一个 isolate 中的 ReceivePort 才能接受到当前类的 SendPort 发送的消息并且处理。
{% endnote %}

### 7. `Future`

> `Future<T>` 类，其表示一个 `T` 类型的异步操作结果。如果异步操作不需要结果，则类型为 `Future<void>`。也就是说首先 Future 是个泛型类，可以指定类型。如果没有指定相应类型的话，则 Future 会在执行动态的推导类型。
>
> 简单来说 Future 就是用来处理异步操作的，异步处理成功了就执行成功的操作，异步处理失败了就捕获错误或者停止后续操作。一个 Future 只会对应一个结果，要么成功，要么失败。
>
> Future 的所有 API 的返回值仍然是一个 Future 对象，所以我们可以很方便的进行链式调用。

{% note secondary %}
**1. 基本使用**

```dart
String _data = '0';

void main() {
  getData();
  print('4-做其他事');
}

void getData() {
  print('1-开始data=$_data');

  Future(() {
    for (int i = 0; i < 100000000; i++) { // 模拟耗时操作
      _data = '网络数据';
    }
    print('2-结束data=$_data');
  });

  print('3-结束data=$_data');
}

运行结果：
flutter: 1-开始data=0
flutter: 3-结束data=0
flutter: 4-做其他事
flutter: 2-结束data=网络数据
```

**2. `async` & `await`**
这两个关键字都是 Dart 语言异步支持的一部分。

- `async`：用来表示函数是异步的，定义的函数会返回一个 Future 对象。
- `await`：后面跟着一个 Future，表示等待该异步任务完成后才会继续往下执行。await 只能出现在异步函数内部，能够让我们可以像写同步代码那样来执行异步任务而不使用回调的方式。

```dart
String _data = '0';

void main() {
  getData2();
  print('4-做其他事');
}

void getData2() async {
  print('1-开始data=$_data');

  // 1.后面的操作必须是异步才能用await
  // 2。当前函数必须是异步函数
  await Future(() {
    for (int i = 0; i < 100000000; i++) {
      _data = '网络数据';
    }
    print('2-结束data=$_data');
  });

  print('3-结束data=$_data');
}

运行结果：
flutter: 1-开始data=0
flutter: 4-做其他事
flutter: 2-结束data=网络数据
flutter: 3-结束data=网络数据
```

**3. `Future.value()`**
创建一个返回指定 value 值的 Future，并且返回 Future。

```dart
void main() {
  futureValueTest();
  print('4-做其他事情');
}

void futureValueTest() async {
  var future = await Future.value(1);
  print(future);
}

运行结果：
flutter: 4-做其他事情
flutter: 1
```

**4. `Future.delay()`**
创建一个延迟执行的 Future，并且返回 Future。

```dart
void main() {
  futterDelayTest();
  print('4-做其他事情');
}

void futterDelayTest() {
  Future.delayed(Duration(seconds: 3), () {
    print("延时3秒执行");
  });
}

运行结果：
flutter: 4-做其他事情
flutter: 延时3秒执行
```

Future 中实现的延时操作通过 Timer 来实现的，在实际开发中，如果只是一个单纯的延时操作，建议使用 Timer，如果需要 `then` 或者 `catchError` 等方法，才使用 Future

```dart
void main() {
  timerTest();
  print('4-做其他事情');
}

void timerTest() {
  Timer(const Duration(seconds: 3), () {
    print("延时3秒执行");
  });
}

运行结果：
flutter: 4-做其他事情
flutter: 延时3秒执行
```

**5. `Future.then()`**
用来注册一个 Future 完成时要调用的回调，并且返回一个 Future 对象。

- 如果 Future 有多个 `then()`，它们也会按照链接的先后顺序同步执行，并共用一个 `event loop`。
- `then()` 比 Future 默认的队列优先级高，`then()` 会在 Future 函数体执行完毕后立刻执行。

```dart
void main() {
  Future(() => print('A')).then((value) => print('A结束'));
  Future(() => print('B')).then((value) => print('B结束'));
}

运行结果：
flutter: A
flutter: A结束
flutter: B
flutter: B结束
```

**6. `Future.catchError()`**
用来注册一个捕捉 Future 的错误的回调，并且返回一个 Future 对象。

> `then()` 在 `catchError()` 前使用，不走处理业务的回调。

```dart
String _data = '0';

void main() {
  getData3();
  print('4-做其他事情');
}

void getData3() async {
  print('1-开始data=$_data');

  Future(() {
    for (int i = 0; i < 100000000; i++) {
      _data = '网络数据';
      throw Exception('网络异常');
    }
    print('1-结束data=$_data');
  }).then((value) {
    print('处理业务');
  }).catchError((e) {
    print('捕获异常');
  });

  print('2-结束data=$_data');
}

运行结果：
flutter: 1-开始data=0
flutter: 2-结束data=0
flutter: 4-做其他事情
flutter: 捕获异常
```

> `then()` 在 `catchError()` 后使用，会走处理业务的回调。

```dart
String _data = '0';

void main() {
  getData4();
  print('4-做其他事情');
}

void getData4() async {
  print('1-开始data=$_data');

  Future(() {
    for (int i = 0; i < 100000000; i++) {
      _data = '网络数据';
      throw Exception('网络异常');
    }
    print('1-结束data=$_data');
  }).catchError((e) {
    print('捕获异常');
  }).then((value) {
    print('处理业务');
  });

  print('2-结束data=$_data');
}

运行结果：
flutter: 1-开始data=0
flutter: 2-结束data=0
flutter: 4-做其他事情
flutter: 捕获异常
flutter: 处理业务
```

**7. `Future.whenComplete()`**
在 Future 完成之后总是会调用，不管是错误导致的完成还是正常执行完毕，并且返回一个 Future 对象。

```dart
void main() {
  Future(() {
    throw '发生错误';
  }).then(print).catchError(print).whenComplete(() => print('whenComplete'));

  Future(() {
    return '没有错误';
  }).then(print).catchError(print).whenComplete(() => print('whenComplete'));
}

运行结果：
flutter: 发生错误
flutter: whenComplete
flutter: 没有错误
flutter: whenComplete
```

**8. `Future.wait()`**
开发中会遇到这样的场景：网络请求 A 和网络请求 B 都完成以后，再执行代码 C，此时可以使用 `Future.wait()`。
`Future.wait()` 会等待多个 Future 完成，并收集它们的结果。

有两种情况：

> 所有 Future 都有正常结果返回。则 Future 的返回结果是所有指定 Future 的结果的集合:

```dart
void main() {
  futureWaitTest();
  print('4-做其他事情');
}

void futureWaitTest() {
  var future1 = new Future(() => '任务1');
  var future2 = new Future(() => '任务2');
  var future3 = new Future(() => '任务3');
  Future.wait([future1, future2, future3]).then(print).catchError(print);
  print('任务添加完毕');
}

运行结果：
flutter: 任务添加完毕
flutter: 4-做其他事情
flutter: [任务1, 任务2, 任务3]
```

> 其中一个或者几个 Future 发生错误，产生了 error。则 Future 的返回结果是第一个发生错误的 Future 的值:

```dart
void main() {
  futureWaitTest();
  print('4-做其他事情');
}

void futureWaitTest() {
  var future1 = new Future(() => '任务1');
  var future2 = new Future(() => throw Exception('任务2异常'));
  var future3 = new Future(() => '任务3');
  Future.wait([future1, future2, future3]).then(print).catchError(print);
  print('任务添加完毕');
}

运行结果：
flutter: 任务添加完毕
flutter: 4-做其他事情
flutter: Exception: 任务2异常
```

**9. `Future.timeout()`**
开发中会遇到这样的场景：网络请求 A 超过 30 秒后抛出超时异常，此时可以使用 `Future.timeout()`。

```dart
void main() {
  futureTimeoutTest();
  print('4-做其他事情');
}

void futureTimeoutTest() {
  Future.delayed(Duration(seconds: 5), () {
    return '网络请求5秒';
  }).timeout(Duration(seconds: 3)).then(print).catchError(print);
}

运行结果：
flutter: 4-做其他事情
flutter: TimeoutException after 0:00:03.000000: Future not completed
```

**10. 《掘金》的一篇文章中有对事件循环 `event loop` 的详细讲解**
地址：[掘金](https://juejin.cn/post/6976136324499636261#heading-15)
{% endnote %}

### 8. Stream
{% note secondary %}
Stream 是一系列异步事件的序列
* 它有一个入口，可以接收指令（数据），这个机器不知道入口什么时候会放东西进来
* 可再机器内部，根据指令，进行数据加工 （转化层，也可做逻辑层）
* 它有一个出口，当内部指令操作完毕后，会有产品从那出来，我们也不知道什么时候产品会从出口出来

**详见《掘金》：**
地址：[掘金](https://juejin.cn/post/6844903686737494023)
{% endnote %}

### 9. await for 如何使用
{% note secondary %}
await for是不断获取stream流中的数据，然后执行循环体中的操作。它一般用在直到stream什么时候完成，并且必须等待传递完成之后才能使用，不然就会一直阻塞。
{% endnote %}  

## Flutter 部分

### 1. Flutter 是如何与原生Android、iOS进行通信的
{% note secondary %}
Flutter 通过 PlatformChannel 与原生进行交互，其中 PlatformChannel 分为三种：
* **BasicMessageChannel** ：用于传递字符串和半结构化的信息。
* **MethodChannel** ：用于传递方法调用（method invocation）。
* **EventChannel** : 用于数据流（event streams）的通信。

同时 Platform Channel 并非是线程安全的
{% endnote %}

### 2. Widget、Element、RenderObject都有什么关系
{% note secondary %}
* **Widget**：仅用于存储渲染所需要的信息。
* **RenderObject**：负责管理布局、绘制等操作。
* **Element**：才是这颗巨大的控件树上的实体。

Widget会被inflate（填充）到Element，并由Element管理底层渲染树。Widget并不会直接管理状态及渲染,而是通过State这个对象来管理状态。Flutter创建Element的可见树，相对于Widget来说，是可变的，通常界面开发中，我们不用直接操作Element,而是由框架层实现内部逻辑。就如一个UI视图树中，可能包含有多个TextWidget(Widget被使用多次)，但是放在内部视图树的视角，这些TextWidget都是填充到一个个独立的Element中。Element会持有renderObject和widget的实例。记住，Widget 只是一个配置，RenderObject 负责管理布局、绘制等操作。
在第一次创建 Widget 的时候，会对应创建一个 Element， 然后将该元素插入树中。如果之后 Widget 发生了变化，则将其与旧的 Widget 进行比较，并且相应地更新 Element。重要的是，Element 不会被重建，只是更新而已。
{% endnote %}

### 3. StatefulWidget 生命周期
{% note secondary %}
* **createState**：该函数为 StatefulWidget 中创建 State 的方法，当 StatefulWidget 被创建时会立即执行 `createState`。`createState` 函数执行完毕后表示当前组件已经在 Widget 树中，此时有一个非常重要的属性 `mounted` 被置为 `true`。
* **initState**：该函数为 State 初始化调用，只会被调用一次，因此，通常会在该回调中做一些一次性的操作，如执行 State 各变量的初始赋值、订阅子树的事件通知、与服务端交互，获取服务端数据后调用 `setState` 来设置 State。
* **didChangeDependencies**：该函数是在该组件依赖的 State 发生变化时会被调用。这里说的 State 为全局 State，例如系统语言 Locale 或者应用主题等，Flutter 框架会通知 widget 调用此回调。类似于前端 Redux 存储的 State。该方法调用后，组件的状态变为 `dirty`，立即调用 `build` 方法。
* **build**：主要是返回需要渲染的 Widget，由于 `build` 会被调用多次，因此在该函数中只能做返回 Widget 相关逻辑，避免因为执行多次而导致状态异常。
* **reassemble**：主要在开发阶段使用，在 debug 模式下，每次热重载都会调用该函数，因此在 debug 阶段可以在此期间增加一些 debug 代码，来检查代码问题。此回调在 release 模式下永远不会被调用。
* **didUpdateWidget**：该函数主要是在组件重新构建，比如说热重载，父组件发生 `build` 的情况下，子组件该方法才会被调用，其次该方法调用之后一定会再调用本组件中的 `build` 方法。
* **deactivate**：在组件被移除节点后会被调用，如果该组件被移除节点，然后未被插入到其他节点时，则会继续调用 `dispose` 永久移除。
* **dispose**：永久移除组件，并释放组件资源。调用完 `dispose` 后，`mounted` 属性被设置为 `false`，也代表组件生命周期的结束。

大致分为四个阶段
1. 初始化阶段，包括两个生命周期函数 `createState` 和 `initState`
2. 组件创建阶段，包括 `didChangeDependencies` 和 `build`
3. 触发组件多次 `build` ，这个阶段有可能是因为 `didChangeDependencies`、`setState` 或者 `didUpdateWidget` 而引发的组件重新 `build` ，在组件运行过程中会多次触发，这也是优化过程中需要着重注意的点
4. 最后是组件销毁阶段，`deactivate` 和 `dispose`
{% endnote %}

## iOS 部分

## 1. assign、retain、copy、weak、strong 的区别
{% note secondary %}
assign：assign 一般用来修饰基础数据类型 （NSInteger、CGFloat、int、float、double、char 等）。因为 assign 声明的属性不会增加引用计数，属性释放后就没有了。

Retain：与 assign 相对，retain 声明后的对象会更改引用计数（被引用，引用计数+1，释放后会-1。即使这个对象本身释放了，只要还有对象在引用它，就会被持有）。只有当引用计数为 0 时，就被 dealloc 析构函数回收。

Copy：最常见使用 copy 来修饰的是 NSString。copy 与 retain 的区别在于 retain 的引用是拷贝指针地址，而 copy 是拷贝对象本身，也就是说 retain 是浅复制，copy 是深复制。如果是浅复制，当修改对象值时，都会被修改，而深复制不会。之所以在 NSString 这类有可变类型的对象上使用，是因为它们有可能和对应的可变类型如 NSMutableString 之间进行赋值操作，为了防止内容被改变，使用 copy 去深复制一份。copy 工作由 copy 方法执行，此属性只对那些实现了 NSCopying 协议的对象类型有效。

Weak：weak 类似于 assign，叫弱引用，不增加引用计数。在防止循环引用时使用（比如父类引用了子类，子类又去引用父类）。IBOutlet、Delegate 一般用的就是 weak，这是因为它们会在类外部被调用，防止循环引用。

Strong：strong 就类似于 retain 了，叫强引用，会增加引用计数，类内部使用的属性一般都是 strong 修饰的，现在 ARC 已经基本替代了 MRC，所以我们最常见的就是 strong 了。
{% endnote %}

## 2. Objective-C 的类可以多重继承么？可以采用多个协议么？
{% note secondary %}
不可以多重继承，可以采用多个协议。
{% endnote %}

## 3. #import 和#include 的区别是什么？＃import<> 跟 #import””有什么区别？
{% note secondary %}
#import 能避免头文件被重复包含的问题:

1. 一般来说，导入 objective c 的头文件时用#import，包含 c/c++头文件时用#include。
   使用 include 要注意重复引用的问题：
   class A，class B 都引用了 class C，class D 若引用 class A 与 class B,就会报重复引用的错误。
2. #import 确定一个文件只能被导入一次，这使你在递归包含中不会出现问题。
   所以，#import 比起#include 的好处就是它避免了重复引用的问题。所以在 OC 中我们基本用的都是 import。
   ＃import<> 包含 iOS 框架类库里的类，#import””包含项目里自定义的类。
{% endnote %}

## 4. Category 是什么？扩展一个类的方式用继承好还是类目好？为什么？
{% note secondary %}
Category 是类目。用类目好，因为继承要满足 a is a b 的关系，而类目只需要满足 a has a b 的关系，局限性更小，你不用定义子类就能扩展一个类的功能，还能将类的定义分开放在不同的源文件里, 用 Category 去重写类的方法，仅对本 Category 有效，不会影响到其他类与原有类的关系。
{% endnote %}

## 5. 延展是什么？作用是什么？
{% note secondary %}
延展（extension）:在自己类的实现文件中添加类目来声明私有方法。
{% endnote %}

## 6. 类实例（成员）变量的@protected ,@private ,@public 声明各有什么含义？
{% note secondary %}
@protected：受保护的，该实例变量只能在该类和其子类内访问，其他类内不能访问。
@private：私有的，该实例变量只能在该类内访问，其他类内不能访问。
@public：共有的，该实例变量谁都可以访问。
{% endnote %}

## 7. id 声明的对象有什么特性？
{% note secondary %}
- 没有 \* 号
- 动态数据类型
- 可以指向任何类的对象(设置是 nil)，而不关心其具体类型
- 在运行时检查其具体类型
- 可以对其发送任何（存在的）消息
{% endnote %}

## 8. 委托是什么？委托和委托方双方的 property 声明用什么属性？为什么？
{% note secondary %}
委托：一个对象保存另外一个对象的引用，被引用的对象实现了事先确定的协议，该协议用于将引用对象中的变化通知给被引用对象。

委托和委托方双方的 property 声明属性都是 assign 而不是 retain

为了避免循环引用造成的内存泄露。

循环引用的问题这样理解：

比如在 main 函数中创建了两个类的对象 A 和 B，现在引用计数都是 1。现在让 A 和 B 互相引用(A 有一个属性是 B 对象，属性说明是 retain；B 有一个属性是 A 对象，属性说明是 retain)，现在两个对象的引用计数都增加了 1，都变成了 2。

现在执行[A release]; [B release]; 此时创建对象的 main 函数已经释放了自己对对象的所有权，但是此时 A 和 B 的引用计数都还是 1，因为他们互相引用了。

这时你发现 A 和 B 将无法释放，因为要想释放 A 必须先释放 B，在 B 的 dealloc 方法中再释放 A。同理，要想释放 B 必须先释放 A，在 A 的 dealloc 方法中再释放 B。所以这两个对象将一直存在在内存中而不释放。这就是所谓的循环引用的问题。要想解决这个问题，一般的方法可以将引用的属性设置为 assign,而不是 retain 来处理。
{% endnote %}

## 9. 浅拷贝和深拷贝区别是什么？
{% note secondary %}
浅层复制：只复制指向对象的指针，而不复制引用对象本身。

深层复制：复制引用对象本身。
{% endnote %}

## 10. 内存管理的几条原则是什么？按照默认法则，哪些关键字生成的对象需要手动释放？哪些情况下不需要手动释放，会直接进入自动释放池？
{% note secondary %}
当使用 new、alloc 或 copy 方法创建一个对象时，该对象引用计数器为 1。如果不需要使用该对象，可以向其发送 release 或 autorelease 消息，在其使用完毕时被销毁。

如果通过其他方法获取一个对象，则可以假设这个对象引用计数为 1，并且被设置为 autorelease，不需要对该对象进行清理，如果确实需要 retain 这个对象，则需要使用完毕后 release。

?如果 retain 了某个对象，需要 release 或 autorelease 该对象，保持 retain 方法和 release 方法使用次数相等。

使用 new、alloc、copy 关键字生成的对象和 retain 了的对象需要手动释放。设置为 autorelease 的对象不需要手动释放，会直接进入自动释放池。
{% endnote %}

## 11. 怎样实现一个单例模式的类，给出思路，不写代码。
{% note secondary %}
首先必须创建一个全局实例，通常存放在一个全局变量中,此全局变量设置为 nil

提供工厂方法对该全局实例进行访问，检查该变量是否为 nil，如果 nil 就创建一个新的实例，最后返回全局实例

全局变量的初始化在第一次调用工厂方法时会在+allocWithZone:中进行，所以需要重写该方法，防止通过标准的 alloc 方式创建新的实例

为了防止通过 copy 方法得到新的实例，需要实现-copyWithZone 方法

只需在此方法中返回本身对象即可，引用计数也不需要进行改变，因为单例模式下的对象是不允许销毁的，所以也就不用保留

因为全局实例不允许释放，所以 retain,release,autorelease 方法均需重写
{% endnote %}

## 12. @class 的作用是什么？
{% note secondary %}
在头文件中， 一般只需要知道被引用的类的名称就可以了。 不需要知道其内部的实体变量和方法，所以在头文件中一般使用@class 来声明这个名称是类的名称。 而在实现类里面，因为会用到这个引用类的内部的实体变量和方法，所以需要使用#import 来包含这个被引用类的头文件。

@class 的作用是告诉编译器，有这么一个类，用吧，没有问题

@class 还可以解决循环依赖的问题，例如 A. h 导入了 B. h，而 B. h 导入了 A. h，每一个头文件的编译都要让对象先编译成功才行

使用@class 就可以避免这种情况的发生
{% endnote %}

## 13. KVC 是什么?KVO 是什么?有什么特点？
{% note secondary %}
KVC 是键值编码，特点是通过指定表示要访问的属性名字的字符串标识符，可以进行类的属性读取和设置

KVO 是键值观察，特点是利用键值观察可以注册成为一个对象的观察者，在该对象的某个属性变化时收到通知
{% endnote %}

## 14. MVC 是什么？有什么特性？
{% note secondary %}
MVC 是一种设计模式，由模型、视图、控制器 3 部分组成。

模型：保存应用程序数据的类，处理业务逻辑的类

视图：窗口，控件和其他用户能看到的并且能交互的元素

控制器：将模型和试图绑定在一起，确定如何处理用户输入的类
{% endnote %}

## 15. id 声明的对象有什么特性？
{% note secondary %}
Id 声明的对象具有运行时的特性，即可以指向任意类型的 objcetive-c 的对象；
{% endnote %}

## 16. RunLoop 的本质是什么？
{% note secondary %}
本质是一个 OC 对象，内部也有 isa 指针。
{% endnote %}

## 17. Objective-C 如何对内存管理的,说说你的看法和解决方法?
{% note secondary %}
Objective-C 的内存管理主要有三种方式 ARC（自动内存计数）、手动内存计数、内存池。
{% endnote %}

## 18. 内存管理的几条原则时什么？
{% note secondary %}
谁申请，谁释放
{% endnote %}

## 19. 那些关键字生成的对象 需要手动释放？
{% note secondary %}
关键字 alloc 或 new 生成的对象需要手动释放
{% endnote %}

## 20 在和 property 结合的时候怎样有效的避免内存泄露？
{% note secondary %}
设置正确的 property 属性，对于 retain 需要在合适的地方释放
{% endnote %}

## 21. 如何对 iOS 设备进行性能测试?
{% note secondary %}
Profile-> Instruments ->Time Profiler
{% endnote %}

## 22. Object－c 的类可以多重继承么？可以实现多个接口么？
{% note secondary %}
Object-c 的类不可以多重继承；可以实现多个接口，通过实现多个接口可以完成 C++的多重继承；
{% endnote %}

## 23. Category 是什么？重写一个类的方式用继承好还是分类好？为什么？
{% note secondary %}
Category 是类别，一般情况用分类好，用 Category 去重写类的方法，仅对本 Category 有效，不会影响到其他类与原有类的关系。
{% endnote %}

## 24. 描述一下 iOS SDK 中如何实现 MVC 的开发模式
{% note secondary %}
MVC 是模型、试图、控制开发模式，对于 iOS SDK，所有的 View 都是视图层的，它应该独立于模型层，由视图控制层来控制。所有的用户数据都是模型层，它应该独立于视图。所有的 ViewController 都是控制层，由它负责控制视图，访问模型数据
{% endnote %}

## 25. Object C 中创建线程的方法是什么？如果在主线程中执行代码，方法是什么？如果想延时执行代码、方法又是什么？
{% note secondary %}
线程创建有三种方法：使用 NSThread 创建、使用 GCD 的 dispatch、使用子类化的 NSOperation,然后将其加入 NSOperationQueue;在主线程执行代码，方法是 performSelectorOnMainThread，如果想延时执行代码可以用 performSelector:onThread:withObject:waitUntilDone
{% endnote %}

## 26. atomic 和 nonatomic 区别
{% note secondary %}
Atomic：原子性，它没有一个如果你没有对原子性进行一个声明（atomic or nonatomic），那么系统会默认你选择的是 atomic。

原子性就是说一个操作不可以被中途 cpu 暂停然后调度, 即不能被中断, 要不就执行完, 要不就不执行. 如果一个操作是原子性的,那么在多线程环境下, 就不会出现变量被修改等奇怪的问题。原子操作就是不可再分的操作，在多线程程序中原子操作是一个非常重要的概念，它常常用来实现一些同步机制，同时也是一些常见的多线程 Bug 的源头。当然，原子性的变量在执行效率上要低些。

Nonatomic：非原子性，是直接从内存中取数值，因为它是从内存中取得数据，它并没有一个加锁的保护来用于 cpu 中的寄存器计算 Value，它只是单纯的从内存地址中，当前的内存存储的数据结果来进行使用。在多线环境下可提高性能，但无法保证数据同步。
{% endnote %}

## 27. OSI（Open System Interconnection）开放式系统互联参考模型 把网络协议从逻辑上分为了 7 层，试列举常见的应用层协议。
{% note secondary %}
注意问的是应用层协议，有些同学直接答了七层模型。

在开放系统互连(OSI)模型中的最高层，为应用程序提供服务以保证通信，但不是进行通信的应用程序本身。

Telnet 协议是 TCP/IP 协议族中的一员，是 Internet 远程登陆服务的标准协议和主要方式。它为用户提供了在本地计算机上完成远程主机工作的能力。

FTP 文件传输协议是 TCP/IP 网络上两台计算机传送文件的协议，FTP 是在 TCP/IP 网络和 INTERNET 上最早使用的协议之一，它属于网络协议组的应用层。

超文本传输协议 (HTTP-Hypertext transfer protocol) 是分布式，协作式，超媒体系统应用之间的通信协议。是万维网（world wide web）交换信息的基础。

SMTP（Simple MailTransfer Protocol）即简单邮件传输协议,它是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式，它帮助每台计算机在发送或中转信件时找到下一个目的地。

时间协议(TIME protocol)是一个在 RFC 868 内定义的网络协议。它用作提供机器可读的日期时间资讯。

DNS 是域名系统 (Domain NameSystem) 的缩写，是因特网的一项核心服务，它作为可以将域名和 IP 地址相互映射的一个分布式数据库。

SNMP(Simple Network ManagementProtocol,简单网络管理协议)的前身是简单网关监控协议(SGMP)，用来对通信线路进行管理。

TFTP（Trivial FileTransfer Protocol,简单文件传输协议）是 TCP/IP 协议族中的一个用来在客户机与服务器之间进行简单文件传输的协议，提供不复杂、开销不大的文件传输服务。端口号为 69。
{% endnote %}

## 28. 网络传输层协议中，基于 TCP/IP 协议和 UDP/IP 的连接有什么区别？
{% note secondary %}
TCP：TransmissionControl Protocol 传输控制协议 TCP 是一种面向连接（连接导向）的、可靠的、基于字节流的运输层（Transport layer）通信协议，由 IETF 的 RFC 793 说明（specified）。

UDP 是 User DatagramProtocol 的简称， 中文名是用户数据包协议，是 OSI 参考模型中一种无连接的传输层协议，提供面向事务的简单不可靠信息传送服务，IETF RFC 768 是 UDP 的正式规范。

面向连接：是指通信双方在通信时，要事先建立一条通信线路，其有三个过程：建立连接、使用连接和释放连接。电话系统是一个面向连接的模式，拨号、通话、挂机；TCP 协议就是一种面向连接的协议。

面向无连接：是指通信双方不需要事先建立一条通信线路，而是把每个带有目的地址的包（报文分组）送到线路上，由系统自主选定路线进行传输。邮政系统是一个无连接的模式，天罗地网式的选择路线，天女散花式的传播形式；IP、UDP 协议就是一种无连接协议。
{% endnote %}

## 29. iOS 中有哪些回调机制，并作简单的比较。
{% note secondary %}
各种回调机制的比较：

1）目标动作对：当两个对象之间有比较紧密的关系时，如视图控制器与其下的某个视图。

2）代理：也叫委托，当某个对象收到多个事件，并要求同一个对象来处理所有事件时。委托机制依赖于某个协议定义的方法来发送消息。

3）通告机制：当需要多个对象或两个无关对象处理同一个事件时。

4）Block：适用于回调只发生一次的简单任务。
{% endnote %}

## 30. Object-C 有多继承吗？没有的话用什么代替？
{% note secondary %}
没有，cocoa 中所有的类都是 NSObject 的子类，多继承在这里是用 protocol 委托代理来实现的? ，ood 的多态特性在 obj-c 中通过委托来实现。
{% endnote %}

## 31. Object-C 有私有方法吗？私有变量呢？
{% note secondary %}
Objective-c 类里面的方法只有两种, 静态方法和实例方法.
@private 可以用来修饰私有变量?在 Objective-C 中，所有实例变量默认都是私有的，所有实例方法默认都是公有的
{% endnote %}

## 32. 堆和栈的区别？
{% note secondary %}
管理方式：对于栈来讲，是由编译器自动管理，无需我们手工控制；对于堆来说，释放工作由程序员控制，容易产生 memory leak。
申请大小：栈：栈是向低地址扩展的数据结构，是一块连续的内存的区域
堆：是向高地址扩展的数据结构，是不连续的内存区域。
分配方式：堆都是动态分配的 ，动态分配由 alloca 函数进行分配
栈的动态分配由编译器进行释放，无需我们手工实现
{% endnote %}

## 33. kvc 和 kvo 的区别？
{% note secondary %}
Kvc：键值编码，是一种间接访问对象的属性，使用字符串来标示属性
Kvo：键值观察机制，提供了观察某一属性变化的方法
{% endnote %}

## 34. 线程是什么？进程是什么？二者有什么区别和联系？
{% note secondary %}
线程，有时称为轻量级进程，是被系统独立调度和 CPU 的基本运行单位。
进程是操作系统中可以并行工作的基本单位。
一个应用程序里至少有一个进程，一个进程里至少有一个线程
{% endnote %}

## 35. 谈谈你对多线程开发的理解？ios 中有几种实现多线程的方法
{% note secondary %}
在一个进程中有多个线程，每个线程有自己单独的任务 优点效率快缺点不安全，耗费资源
有三种： 第一种，使用@synchronized(self) 第二种，使用 GCD 第三种，使用 NSOperationQueue
{% endnote %}

## 36. 线程同步和异步的区别？IOS 中如何实现多线程的同步？
{% note secondary %}
一个进程启动的多个不相干线程，它们相互之间关系为异步。

同步的话指的是多线程同时操作一个数据这个时候需要对数据添加保护这个保护就是线程的同步。

用 GCD 中的串行队列来解释多线程的同步，也就是队列中的任务为串行，它们各自对相邻的任务有依赖性，如果任务 1 不完成，那么任务 2 就不会开始，这就是同步
{% endnote %}

## 37. 自动释放池是什么,如何工作
{% note secondary %}
当您向一个对象发送一个 autorelease 消息时，Cocoa 就会将该对象的一个引用放入到最新的自动释放池。它仍然是个正当的对象，因此自动释放池定义的作用域内的其它对象可以向它发送消息。当程序执行到作用域结束的位置时，自动释放池就会被释放，池中的所有对象也就被释放。
{% endnote %}

## 38. 说说响应链
{% note secondary %}
事件响应链。包括点击事件，画面刷新事件等。在视图栈内从上至下，或者从下之上传播。

可以说点事件的分发，传递以及处理。具体可以去看下 touch 事件这块。
{% endnote %}

## 39. frame 和 bounds 有什么不同？
{% note secondary %}
Frame 指的是：该 view 在父 view 坐标系统中的位置和大小。（参照点是父亲的坐标系统）

Bounds 指的是：该 view 在本身坐标系统中 的位置和大小。（参照点是本身坐标系统）
{% endnote %}

## 40. 方法和选择器有何不同？
{% note secondary %}
Selector 是一个方法的名字，method 是一个组合体，包含了名字和实现.
{% endnote %}

## 41. 指针与地址的区别?
{% note secondary %}
1、指针意味着已经有一个指针变量存在,他的值是一个地址,指针变量本身也存放在一个长度为四个字节的地址当中,而地址概念本身并不代表有任何变量存在.

2、 指针的值,如果没有限制,通常是可以变化的,也可以指向另外一个地址.

3、地址表示内存空间的一个位置点,他是用来赋给指针的,地址本身是没有大小概念,指针指向变量的大小,取决于地址后面存放的变量类型.
{% endnote %}

## 42. 线程、队列的关系? 一个线程是否可能存在于两个队列?
{% note secondary %}
线程是系统调度的最小任务单位，队列是存放管理任务单位的数据结构。
{% endnote %}

## 43. 队列一定会创建线程吗?
{% note secondary %}
答：不，同步执行方式是不创建新线程的，就在当前线程。
线程按执行方式分为同步、异步，按队列管理分为串行并行，这样有四种组合，加上常说的主线程主队列，那么结合执行方式就有六种组合。
同步串行，不创建线程，所以还是在当前线程一个一个做
同步并行，不创建线程，所以就算是并行，也还是在当前线程一个一个做
异步串行，开辟多一条线程，任务在新开辟的一条线程里面一个一个做
异步并行，开辟多条线程，任务在新开辟的线程里面一起做
同步主队，阻塞
异步主队，同异步串行，因为主队就是串行，但是不开辟新线程，因为主线程是全局的单例的
{% endnote %}

## 44. 队列是否可以无限制创建?
{% note secondary %}
不能，队列也是对象，要占用内存，受限于硬件资源，不能无限制创建。
{% endnote %}

## 45. gcd 的使用，能不能取消？
{% note secondary %}
dispatch_block_cancel 可以取消尚未执行的任务。已经在运行的，用代码中断
{% endnote %}

## 46. 如何进行线程保活
{% note secondary %}
想让线程不死掉的话，需要为线程添加一个 RunLoop
{% endnote %}

## 47. GCD、NSOperation 区别, 功能方法区别.
{% note secondary %}
NSThread 是早期的多线程解决方案，实际上是把 C 语言的 PThread 线程管理代码封装成 OC 代码。
GCD 是取代 NSThread 的多线程技术，C 语法+block。功能强大。
充分利用多核，效率最高

NSOperationQueue 是把 GCD 封装为 OC 语法，额外比 GCD 增加了几项新功能。
最大线程并发数
取消队列中的任务
暂停队列中的任务
可以调整队列中的任务执行顺序，通过优先级
线程依赖
NSOperationQueue 支持 KVO。 这就意味着你可以观察任务的状态属性。
但是 NSOperationQueue 的执行效率没有 GCD 高，所以一半情况下，我们使用 GCD 来完成多线程操作。
{% endnote %}

## 48. GCD、NSOperationQueue 的区别
{% note secondary %}
GCD 是取代 NSThread 的多线程技术，C 语法+block。功能强大。
充分利用多核，效率最高

NSOperationQueue 是把 GCD 封装为 OC 语法，额外比 GCD 增加了几项新功能。
最大线程并发数
取消队列中的任务
暂停队列中的任务
可以调整队列中的任务执行顺序，通过优先级
线程依赖
NSOperationQueue 支持 KVO。 这就意味着你可以观察任务的状态属性。
但是 NSOperationQueue 的执行效率没有 GCD 高，所以一半情况下，我们使用 GCD 来完成多线程操作。
{% endnote %}

## 49. group 如何实现 barrier 类似的功能?
{% note secondary %}
Barrier 栅栏功能，栅栏前不管多少个异步都要执行完毕，才会执行栅栏后面的操作。
可以尝试用信号量来实现，例如 A、B、C、barrier、D 并发，但是希望 ABC 完成后 D 才开始。
设定线程信号量最大值为 3，ABC 先执行，等 ABC 都执行完，D 才开始
{% endnote %}

## 50. instance（实例对象）、class（类对象）、meta-class（元类对象）分别储存了什么信息？为什么要设计元类？
{% note secondary %}
Instance 对象在内存中存储的信息包括
（1）isa 指针 （2）其它成员变量
Class 对象在内存中存储的信息主要包括：
（1）isa 指针
（2）superclass 指针
（3）类的属性信息（@property）、类的对象方法信息（instance method）
（4）类的协议信息（protocol）、类的成员变量信息（ivar）
Meta-class 对象和 class 对象的内存结构是一样的，但是用途不一样，在内存中存储的信息主要包括
（1）isa 指针
（2）superclass 指针
（3）类的类方法信息（class method）
为什么存在元类的设计？从面向对象的设计理念来说，万物皆对象，类也是对象，描述类的元类，存在的目的就是自上而下的逻辑自洽，也方便 message 的传递。
{% endnote %}

## 51. KVO 实现原理？
{% note secondary %}
KVO 是根据 iOS runtime 实现的，当监听某个对象（\_kvoTest）的某个属性时，KVO 会创建这个对象的子类，并重写我们监听的属性（keyPath）的 set 方法，具体实现可能是下面这个样子。

当观察对象时，KVO 机制动态创建一个新的名为：NSKVONotifying*对象名 的新类，该类继承自目标对象的本类，且 KVO 为 NSKVONotifying*对象名 重写观察属性的 set 方法。在这个过程，被观察对象的 isa 指针从指向原来的对象，被 KVO 机制修改为指向系统新创建的子类 NSKVONotifying*对象名 类，来实现当前类属性值改变的监听，这也就是前面所说的“黑魔法”；我还试了一下，创建一个新的名为“NSKVONotifying*对象名”的类，发现系统运行到注册 KVO 的代码时，iOS10 及以下会崩溃，iOS11 下控制台打印警告：存在同名类，无法进行 KVO 监听了。
{% endnote %}

## 52. isa 指针是什么？什么是 TaggedPointer 的优化？
{% note secondary %}
对象. isa -> 类. super -> 父类. super -> 根类. super -> nil
类. isa -> 元类. super -> 父元类. super -> 根元类. super -> 根类. super -> nil
元类. isa = 父元类. isa = 根元类. isa = 根元类
在 arm64 架构之前，isa 就是一个普通指针，存储着 Class、Meta-Class 对象的内存地址；从 arm64 架构开始，变成了一个共用体 union 结构，还使用位域来存储更多的信息。

Tagged Pointer 的优化：
Tagged Pointer 专门用来存储小的对象，例如 NSNumber, NSDate, NSString。
Tagged Pointer 指针的值不再是地址了，而是真正的值。所以，实际上它不再是一个对象了，它只是一个披着对象皮的普通变量而已。所以，它的内存并不存储在堆中，也不需要 malloc 和 free。
在内存读取上有着 3 倍的效率，创建时比以前快 106 倍。
例如：1010，其中最高位 1xxx 表明这是一个 tagged pointer，而剩下的 3 位 010，表示了这是一个 NSString 类型。010 转换为十进制即为 2。也就是说，标志位是 2 的 tagger
pointer 表示这是一个 NSString 对象。
{% endnote %}

## 53. isa 指针里面都存了什么，32 和 64 位分别讲一下
{% note secondary %}
在 ARM 32 位的时候，isa 的类型是 Class 类型的，直接存储着实例对象或者类对象的地址；
在 ARM64 结构下，isa 的类型变成了共用体(union)，使用了位域去存储更多信息
{% endnote %}

## 54. OC 是否支持重载? 为什么?
{% note secondary %}
不完全支持，参数个数不同的函数重载可以说是支持。但是参数相同、函数名相同的 编译不通过
{% endnote %}

## 55. IMP、SEL Method 都表示什么意思?
{% note secondary %}
SEL，方法选择器，本质上是一个 C 字符串
IMP，函数指针，函数的执行入口
Method，类型，结构体，里面有 SEL 和 IMP
{% endnote %}

## 56. class 的底层结构是什么样的？
{% note secondary %}
typedef struct objc_class \*Class;
objc_class 里面有 isa 指针、superclass 指针、方法缓存、具体的类信息
{% endnote %}

## 57. method_t 里包含什么？
{% note secondary %}
``` objective-c
struct method_t{
SEL name; //函数名
const char \*types; //编码（返回值类型、参数类型）
IMP imp; //指向函数的指针（函数地址）
};
```
{% endnote %}

## 58. super 关键字的本质是什么？
{% note secondary %}
Super：是编译器指示符，仅仅是一个标志,并不是指针，仅仅是标志的当前对象去调用父类的方法，本质还是当前对象调用。
{% endnote %}

## 59. OC 的消息机制有几步？
{% note secondary %}
消息发送：刚调用 objc_msgSend 函数后，内部的一些处理逻辑。会涉及到 cache list 和 method list 等。
动态方法解析：允许开发者动态创建方法。
消息转发：进入消息转发阶段。
{% endnote %}

## 60. 如何防止类似 unrecognized selector 的错误？\_objc_msgForward 能干什么？
{% note secondary %}
消息转发机制中三大步骤：消息动态解析、消息接受者重定向、消息重定向。通过这三大步骤，可以让我们在程序找不到调用方法崩溃之前，拦截方法调用。
{% endnote %}

## 61. runtime 有哪些应用？方法替换（method - Swizzling）有什么缺点？如何安全的进行方法替换？
{% note secondary %}
1、逆向。2、分类属性附加。3、预防崩溃等。4、方法的交换 swizzling。5、快速定义归档和解档属性。
缺点：
1、找不到真正的方法归属
例如数组越界，你以为替换的是 NSArray 的方法，事实上是\_NSArrayI 的方法
2、多次进行方法交换，会将方法替换为原来的实现
解决：利用单利进行限制，只进行一次方法交换
3、交换的旧方法，子类未实现，父类实现
出现的问题：父类在调用旧方法时，会崩溃
解决方法：先进行方法添加，如果添加成功则进行替换，反之，则进行交换
4、进行交换的方法，子类、父类均未实现
出现的问题：出现死循环 解决方法：如果旧方法为 nil，则替换后将 swizzeldSEL 复制一个不做任何操作的空实现。
5、类方法–类方法存在元类中。
答：找错替换的目标了
{% endnote %}

## 62. Swizzle 时, 我不想替换父类, 只想替换子类,怎么办?
{% note secondary %}
先判断子类是否存在该方法，不存在就添加方法，这样 Swizzle 时候因为方法已经存在了，就不会上溯到父类
{% endnote %}

## 63. 堆和栈的区别是什么?
{% note secondary %}
堆：先进先出，栈：先进后出
从管理方式区分，堆：手动释放内存，栈：自动释放内存
从空间大小，堆：空间大速度慢，栈：空间小速度快
{% endnote %}

## 64. weak 如何把 对象重制为 nil
{% note secondary %}
runtime 维护了一个 Weak 表，weak_table_t
用于存储指向某一个对象的所有 Weak 指针。Weak 表其实是一个哈希表， key 是所指对象的地址，value 是 weak 指针的地址的数组。
在对象回收的时候，就会在 weak 表中进行搜索，找到所有以这个对象地址为键值的 weak 对象，从而置 nil。
{% endnote %}

## 65. AutoReleasePool（自动释放池） 的底层实现是什么？他怎么实现及时释放的？子线程的释放时机是怎么样的？
{% note secondary %}
一个线程的 autoreleasepool 就是一个指针栈。栈中存放的指针指向加入需要 release 的对象。
当自动释放池销毁时自动释放池中所有对象作 release 操作。
每个线程创建的时候就会创建一个 autoreleasepool，所以子线程的 autorelease 对象，要么在子线程中设置 runloop 清除，要么在线程退出的时候，清空 autoreleasepool
{% endnote %}

## 66. ARC 下哪些情况会造成内存泄漏
{% note secondary %}
1、Delegate 循环引用
2、Block 循环引用
3、performSelector 动态调用
{% endnote %}

## 67. [mutablearry alloc]init 和 [nsmublearray array]有什么区别
{% note secondary %}
[[NSMutableArray alloc]init] alloc 分配内存，init 初始化，需要手动释放
[NSMutableArray array] 不需要手动 release，遵循 autoreleasepool 机制
在 ARC（自动引用计数）中两种方式并没什么区别
{% endnote %}

## 68. 结构体中为什么不能使用 oc 对象
{% note secondary %}
结构体，生效周期是编译时，oc 对象是运行时，结构体在栈区，oc 对象在堆区
{% endnote %}

## 69. 我们在开发中使用文件的. mm 是基于什么原因?
{% note secondary %}
C++混编
{% endnote %}

## 70. 你知道 iOS 有哪些锁？性能分别怎么样？
{% note secondary %}
11 种。
性能从强到弱
OSSpinLock,
信号量,
pthread_mutex,
NSLock,pthread_rwlock,
os_unfair_lock,
pthread_mutex(recursive),
NSRecursiveLock,
NSConditionLock,
@sychronized
{% endnote %}

## 71. NSTimer、CADisplayLink、dispatch_source_t 的优劣
{% note secondary %}
NSTimer：
优点： 方便使用 缺点： 计时不精确，容易造成循环引用
CADisplayLink：
优点： 只要设备屏幕刷新频率保持在 60fps，那么其触发时间上是最准确的。 缺点：
由于依托于屏幕刷新频率，如果 CPU 任务过重，那么会影响屏幕刷新，触发事件也会受到相应影响。
dispatch_source_t：
优点：
非常精确，要想达到百分比精确需要单独拥有一个队列，只处理 time 相关的任务，并且 time 的回调处理时长不能操过设置的时间间隔。
缺点： 不受 RunLoop 的影响，需要注意内存管理
{% endnote %}

## 72. 自旋锁和互斥锁怎么选择？
{% note secondary %}
自旋锁：
1、时间短
2、加锁的代码（临界区）经常被调用，但竞争情况很少发生
3、CPU 资源不紧张
4、多核
互斥锁：
1、时间长
2、单核
3、临界区有 IO 操作
4、临界区代码复杂或者循环量大
5、临界区竞争非常激烈
{% endnote %}

## 73. NSNotificationCenter 跨线程及底层结构是怎样的？
{% note secondary %}
总结：接收通知和发送通知时所在线程一致，和监听时所在线程无关。
{% endnote %}

## 74. atomic 与@synchroize 原理
{% note secondary %}
atomic 原子性，setter 和 getter 加了自旋锁（实则是互斥锁）。@synchronized 是一个互斥递归锁。
{% endnote %}

## 75. HTTP、HTTPS 区别?
{% note secondary %}
HTTPS 是 HTTP 的安全版，在 HTTP 的基础上加入 SSL 层，对数据传输进行加密和身份验证
{% endnote %}

## 76. 造成 tableView 卡顿的原因有哪些？
{% note secondary %}
1. 最常用的就是 cell 的重用
   注册重新标识符 如果是重用 cell 时，每当 cell 显示到屏幕上时，就会重新创建一个新的 cell;如果有很多数据的时候，就会堆积很多 cell。
   如果重用 cell,就为 cell 创建一个 ID,每当需要显示 cell 的时候,都会先去缓冲池中寻找可循环的 cell,如果没有再重新创建 cell.
2. 避免 cell 的重新布局
   cell 的布局填充等操作比较耗时,一般创建时就布局好,如可以将 cell 单独放到一个自定义类,初始化时就布局好.
3. 提前计算并缓存 cell 的属性及内容
   当我们创建 cell 的数据源方法时，编译并不是先创建 cell 再定 cell 的 高度，
   是先根据内容依次确定每个 cell 的高度,高度确定后，再创建要显示的
   cell，滚动时，每当 cell 进入都会计算高度，提前估算高度告诉编译,编译知道高度后,紧接着就会创建 cell，这时再调高度的具体计算方法,这样可以不浪费时间去计算显示以外的 cell
4. 减少 cell 中控件的数,尽量使 cell 得布局相同,同格的 cell 可以使的重 标识符,初始化时添加控件,适当的可以先隐藏.
5. 要使用 ClearColor,背景,透明度也要设置为 0 渲染耗时较大
6. 使用局部刷新 ,如果只是新某组的话，使 reloadSection 进行局部刷新
7. 加载网络数据,下载图,使用异步加载,并缓存
8. 少使 addView 给 cell 动态添加 view
9. 按需加载 cell，cell 滚动很快时，只加载范围内的 cell
10. 要实现的代理方法，tableView 只遵守两个协议,不用的代理方法可以不写.
11. 缓存 :estimatedHeightForRow 能和 HeightForRow 的 layoutIfNeed 同时存在，这两者同时存在才会出现“窜动”的 bug。所以我的建 议是:只要是固定 就写预估 来减少 调
    次数提升性能。如果是动态的就要写预估算法 ，每个的缓存字典来减少代码的调用次数即可.
12. 要做多余的绘制 作。 在实现 drawRect:的时候，它的 rect 参数就是需要绘制的区域，这个区域之外的 需要进 绘制. 如上 中，就可以 CGRectIntersectsRect、CGRectIntersection 或
    CGRectContainsRect 判断是否需要绘制 image 和 text，然后再调绘制方法。
13. 预渲染图像. 当新的图像出现时，仍然会有短暂的停顿现象。解决的办法就是在 bitmap context 先将其画 遍，导出成 UIImage 对象，然后再绘制到屏幕;
14. 使用正确的数据结构来存储数据。
{% endnote %}

## 77. APP 启动时间应从哪些方面优化？
{% note secondary %}
App 启动分为两种：
冷启动（Cold Launch）：从零开始启动 app
热启动（Warm Launch）：app 已在内存中，在后台存活，再次点击图标启动 app
启动时间的优化，主要是针对冷启动进行优化

冷启动可以概括为 3 大阶段：
dyld
runtime
main

按照不同的阶段优化
dyld：
减少动态库、合并一些动态库（定期清理不必要的动态库）
减少 objc 类、分类的数量、减少 selector 数量（定期清理不必要的类、分类）
减少 C++虚构函数
Swift 尽量使用 struct

runtime：
使用+initialize 方法和 dispatch_once 取代所有的**attribute**((constructor))、C++静态构造器、Objc 的+load 方法

main：
在不影响用户体验的前提下，尽可能将一些操作延迟，不要全部都放在 finishLaunching 方法中
按需加载
{% endnote %}

## 78. 卡顿掉帧优化策略？
{% note secondary %}
由于是因为 CPU 和 GPU 工作耗时导致的卡顿掉帧，所以可以从 CPU 和 GPU 两方面来进行优化，减轻两者的耗时工作。
CPU：将 CPU 的工作放置到子线程完成。如对象的创建、调整、销毁；layout 布局计算、文本计算；文本的异步绘制、图片编解码。
GPU：避免离屏渲染。视图的圆角设置、阴影、蒙版、光栅化都会造成离屏渲染增加 GPU 工作量，可通过 CPU 的异步绘制机制来完成这类操作，从而减轻 GPU 压力。
{% endnote %}

## 79. sdwebimage 原理
{% note secondary %}
1. 当我门需要获取网络图片的时候，我们首先需要的便是 URl，没有 URl 什么都没有，获得 URL 后我们 SDWebImage 实现的并不是直接去请求网路，而是检查图片缓存中有没有和 URl 相关的图片，如果有则直接返回 image，如果没有则进行下一步。

2. 当图片缓存中没有图片时，SDWebImage 依旧不会直从网络上获取，而是检查沙盒中是否存在图片，如果存在，则把沙盒中对应的图片存进 image 缓存中，然后按着第一步的判断进行。

3. 如果沙盒中也不存在，则显示占位图，然后根据图片的下载队列缓存判断是否正在下载，如果下载则等待，避免二次下载。如果不存则创建下载队列，下载完毕后将下载操作从队列中清除，并且将 image 存入图片缓存中。

4. 刷新 UI（当然根据实际情况操作）将 image 存入沙盒缓存。
{% endnote %}

## 80. iOS 数据存储常用的 5 种方式
{% note secondary %}
1. xml 属性列表(plist)归档.
2. preference(偏好设置).
3. nskeyedarchiver 归档.
4. sqlite3
5. core data
{% endnote %}
