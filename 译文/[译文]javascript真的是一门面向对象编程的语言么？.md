原文[ https://dzone.com/articles/is-javascript-a-true-oop-language ]
　　我知道，这个话题已经被讨论过无数次，但是，它是当前总会被提及的话题。每当一个使用Java或C#或者其他面向对象开发语言的开发者接触JavaScript的时候，他总会抱怨，会说JavaScript太混乱、没有类型、结构也不好，还有很多奇奇怪怪的地方，它的对象支持也是微乎其微，因此他绝对不是一个面向对象编程的语言。
　　有些抱怨还是可以接受的，但有一部分确实带有偏见。例如JavaScript没有类型的声明，那么它就不是OOP语言。关于后一点，在确认JavaScript是不是面向对象编程之前，你应该问一下你自己：**是什么让一门编程语言成为面向对象的编程语言**。
###什么是OOP
　　OOP模式不是基于正式的标准规范，没有一个技术文档定义了OOP它是什么，它不是什么。OOP定义主要是基于早期研究人员发表的论文中的常识，如Kristen Nygaard, Alan Kays, William Cook等人。已经有很多尝试去定义OOP和普遍被接受的定义方式对编程语言按照面向对象进行分类，主要是基于两点要求：
- 它具有以对象形式将一个问题进行建模的能力。
- 它对于编程中模块化和代码重用原则的支持度。

　　为了满足第一个要求，一个语言必须允许开发者使用对象来描述现实并定义对象之间的关系，如下所示：
- **关联**：对象引用另一个独立对象的能力。
- **聚合**：对象嵌入一个或多个独立对象的能力。
- **组合**：对象嵌入一个或多个依赖对象的能力。

　　一般来说，如果语言支持以下原则，则满足第二个要求：
- **封装**：这是集中在单个实体的数据和操纵它的代码，隐藏其内部细节的能力。
- **继承**：这是一种对象从一个或多个其他对象获取某些或所有要素的机制。
- **多态**：这是根据数据类型或结构不同地处理对象的能力。

符合这些要求的，通常我们就可以把一个编程语言分类为面向对象。
###JavaScript & OOP
现在我们知道一个OOP语言是是什么样子的了，那么我们能够证明JavaScript是一个OOP语言么？让我们来试一下吧
接下来，展示一下JavaScript对象实现关联、聚合、组合是很轻松的。
请参考以下代码：
```

var johnSmith = {
 firstName: "John",
 lastName: "Smith",
 address: { //Composition  **组合**
 street: "123 Duncannon Street",
 city: "London",
 country: "United Kingdom"
 }
};
var nickSmith = {
 firstName: "Nick",
 lastName: "Smith",
 address: { //Composition **组合**
 street: "321 Oxford Street",
 city: "London",
 country: "United Kingdom"
 }
};
johnSmith.parent = nickSmith; //Association **关联**
var company = {
 name: "ACME Inc.",
 employees: []
};
//Aggregation **聚合**
company.employees.push(johnSmith);
company.employees.push(nickSmith);
```
您可以从上面代码中找到一个组合（address属性）的示例，一个关联（parent 属性）的示例和一个聚合示例（employees属性）。
　　**封装**，JavaScript对象是拥有数据和方法的实例，但它们没有高级原生方法隐藏内部细节。 JavaScript对象不关心隐私。 一不小心，可能导致所有属性和方法都可以公开访问。但是，我们可以通过一些技术来定义对象的内部状态，防止对象的外部访问：通过使用getter和setter来利用闭包。
　　通过所谓的**原型继承**，JavaScript从它最基本层次上支持了**继承**。 即使一些开发人员认为它有点简单，但JavaScript的继承机制是完全有效的，也让您获得与大多数公认的OOP语言相同的结果。 无论如何，JavaScript都有一个机制：“一个对象从一个或多个其他对象获取一些或所有的功能”，这是继承。
　　如何让JavaScript具有**多态性**，似乎让挑战更加困难了，因为许多人把这个概念与数据类型联系起来。 实际上，多态性涉及编程语言的许多方面，它不仅与OOP语言有关。 通常它涉及很多项，例如泛型，重载和结构子类型。 所有这些东西对于一个JavaScript这种“简单”和弱类型的语言来说似乎太多了。然而并不是：在JavaScript中，我们可以通过几种方式实现不同类型的多态，也许我们写代码的时候在不知不觉中已经做了很多次。
###没有 类 的OOP
"好吧，但是JavaScript没有类啊" ***（反对JavaScript为OOP的语气说）***
许多开发者因为JavaScript缺乏类的概念而没有把它看作一种真正面向对象语言，因为它不符合OOP的硬性要求。
　　但是，我们可以看到，我们的非正式定义中没有明确的要求具备类。 特性和原理才是对象所明确要求具备的。 类不是真正必须具备的，但它们有时是一种很方便的方法，让我们可以抽象出具有公共属性的对象。 因此，如果像JavaScript这样的语言，即使没有类，只要支持对象，那么也是面向对象的语言。

PS：ES6让JavaScript越来越强健，作者说的一些缺点，在ES6中慢慢都有补充。









