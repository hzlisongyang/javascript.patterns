# 对象创建模式

在JavaScript中申明对象很容易——可以通过使用对象直接量或者构造函数。本章将在此基础上介绍一些常用的对象创建模式。

JavaScript语言本身简单、直观，通常也没有其他语言那样的语法特性：命名空间、模块、包、私有属性以及静态成员。本章将介绍一些常用的模式，以此实现这些语法特性。

我们将对命名空间、依赖声明、模块模式以及沙箱模式进行初探——它们帮助更好地组织应用程序的代码，有效地减轻全局污染的问题。除此之外，还会对包括：私有和特权成员、静态和私有静态成员、对象常量、链以及类式函数定义方式在内的话题进行讨论。  

## 命名空间模式（Namespace Pattern）

命名空间可以帮助减少全局变量的数量，与此同时，还能有效地避免命名冲突、名称前缀的滥用。

JavaScript默认语法并不支持命名空间，但很容易可以实现此特性。为了避免产生全局污染，你可以为应用或者类库创建一个（通常就一个）全局对象，然后将所有的功能都添加到这个对象上，而不是到处申明大量的全局函数、全局对象以及其他全局变量。

看如下例子：

	// BEFORE: 5 globals
	// Warning: antipattern
	// constructors function Parent() {} function Child() {}
	// a variable
	var some_var = 1;
	// some objects
	var module1 = {}; module1.data = {a: 1, b: 2}; var module2 = {};

可以通过创建一个全局对象（通常代表应用名）来重构上述这类代码，比方说， MYAPP，然后将上述例子中的函数和变量都变为该全局对象的属性：

	// AFTER: 1 global
	// global object var MYAPP = {};
	// constructors
	MYAPP.Parent = function () {}; MYAPP.Child = function () {};
	// a variable MYAPP.some_var = 1;
	// an object container MYAPP.modules = {};
	// nested objects
	MYAPP.modules.module1 = {}; MYAPP.modules.module1.data = {a: 1, b: 2}; MYAPP.modules.module2 = {};

这里的MYAPP就是命名空间对象，对象名可以随便取，可以是应用名、类库名、域名或者是公司名都可以。开发者经常约定全局变量都采用大写（所有字母都大写），这样可以显得比较突出（不过，要记住，一般大写的变量都用于表示常量）。

这种模式是一种很好的提供命名空间的方式，避免了自身代码的命名冲突，同时还避免了同一个页面上自身代码和第三方代码（比如：JavaScript类库或者小部件）的冲突。这种模式在大多数情况下非常适用，但也有它的缺点：

* 代码量稍有增加；在每个函数和变量前加上这个命名空间对象的前缀，会增加代码量，增大文件大小
* 该全局实例可以被随时修改
* 命名的深度嵌套会减慢属性值的查询

本章后续要介绍的沙箱模式则可以避免这些缺点。


