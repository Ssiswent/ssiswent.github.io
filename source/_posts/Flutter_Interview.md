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

{% endnote %}