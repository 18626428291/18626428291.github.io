### 对象 <!-- {docsify-ignore} -->

**文档更新日期: {docsify-updated}**





#### 对象的构成

---

在 ECMAScript 中，对象由特性（attribute）构成，特性可以是原始值，也可以是引用值。如果特性存放的是函数，它将被看作对象的方法（method），否则该特性被看作对象的属性（property）。





---

**在 ECMAScript 中，所有对象并非同等创建的。**

**一般来说，可以创建并使用的对象有三种：本地对象、内置对象和宿主对象。**



#### 本地对象

---

ECMA-262 把本地对象（native object）定义为“独立于宿主环境的 ECMAScript 实现提供的对象”。简单来说，本地对象就是 ECMA-262 定义的类（引用类型）。

它们包括：

- Object
- Function
- Array
- String
- Boolean
- Number
- Date
- RegExp
- Error
- EvalError
- RangeError
- ReferenceError
- SyntaxError
- TypeError
- URIError







#### 内置对象

---

ECMA-262 把内置对象（built-in object）定义为“由 ECMAScript 实现提供的、独立于宿主环境的所有对象，在 ECMAScript 程序开始执行时出现”。这意味着开发者不必明确实例化内置对象，它已被实例化了。

ECMA-262 只定义了两个内置对象，即 Global 和 Math （它们也是本地对象，根据定义，每个内置对象都是本地对象）。





#### 宿主对象

---

所有非本地对象都是宿主对象（host object），即由 ECMAScript 实现的宿主环境提供的对象。

所有 BOM 和 DOM 对象都是宿主对象。



