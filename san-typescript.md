# 三、Typescript

## 1、ts有哪些数据类型

1）基本类型：number、string、boolean、Symbol、null、undefined

2）复合类型：enum、array（也可用如number\[]、Array\<number>表示）、tuple

3）函数类型：function、void、any

4）对象类型：object、interface（接口）

5）高级类型：Union types（联合类型）、Intersection types（交叉类型）

【延伸】对于class，class关键字本身不是数据类型，但定义的类既可以用作构造对象的模板（作为值），也可以用作类型注解（作为类型）。这种设计增强了 TypeScript 的面向对象编程能力，使其在类型安全性和代码组织上更为强大。

## 2、常量枚举

常量枚举（Const Enum）是 TypeScript 中一种特殊的枚举类型，它提供了一种在编译阶段内联枚举成员值的方式，以减少编译输出的大小和运行时的开销（但不适用于需要运行时枚举信息的情况）。常量枚举与普通枚举的主要区别在于它们在编译后的处理方式，常量枚举在编译过程中会被完全移除，并且所有对常量枚举成员的引用都会被替换为相应的字面量值。

（1）定义一个常量枚举

```typescript
const enum Directions {
	Up,
	Down,
	Left,
	Right
}
```

（2）常量枚举的特点

* 编译时解析：常量枚举在编译时会被解析为具体的字面量值，不会包含枚举定义的代码，有助于减少最终生成的 JavaScript 文件的大小。
* 无运行时开销：由于常量枚举成员在编译时就被替换为具体的值，因此使用常量枚举不会产生运行时的开销。这对性能敏感的应用尤其有益。
* 不支持反向映射：与普通枚举不同，常量枚举不支持从枚举值到枚举名称的反向映射，因为常量枚举在编译后就不再存在。
* 受限的使用场景：由于常量枚举在编译阶段被内联和移除，因此它们不能用于所有与普通枚举兼容的场景。例如，你不能在运行时动态地访问常量枚举的成员列表或使用非常量枚举特有的特性。

（3）使用常量枚举

常量枚举的使用方式与普通枚举相同。你可以直接使用枚举成员：

```typescript
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

在编译后，这段代码中对常量枚举成员的引用会被替换为它们对应的字面量值：let directions = \[0, 1, 2, 3];

## 3、枚举和常量枚举区别

在 TypeScript 中，枚举（Enum）和常量枚举（Const Enum）是用来定义一组命名常量的方法，它们都可以使代码更清晰、更易维护。然而，两者之间存在一些关键的区别，主要体现在编译结果和使用场景上。

枚举是 TypeScript 特有的一种数据类型，用于定义一组命名的数值常量。使用枚举可以清晰地表达意图或创建一组有区别的用例。枚举在编译为 JavaScript 代码后会保留，这意味着枚举在运行时是存在的，因此可以在运行时访问枚举的名字和值。例如：

```typescript
enum Color { Red, Green, Blue }; 
let c: Color = Color.Green;
```

编译后的 JavaScript 代码大致如下：

```typescript
var Color;
(function(Color){
	Color[Color[“Red”] = 0] = “Red”;
	Color[Color[“Green”] = 1] = “Green”;
	Color[Color[“Blue”] = 2] = “Blue”;
})(Color || Color = {});
let c = Color.Green;
```

常量枚举与普通枚举的主要区别在于，它会在编译时被完全删除，编译器会将常量枚举的成员直接替换为对应的字面量值。这种行为使得常量枚举在性能上有所提升，因为它避免了额外的对象查找开销并减少了最终生成的代码量。

```typescript
const enum Color { Red, Green, Blue };
let c: Color = Color.Green;
```

编译后的 JavaScript 代码大致如下：

```typescript
let c = 1; // 直接使用了值，而不是枚举名
```

区别总结：

* 编译结果：常量枚举在编译后会被完全移除，编译器会将它们的用法直接替换为相应的字面量值；而普通枚举则会被编译成对象，保留在最终的 JavaScript 代码中。
* 运行时性能：由于常量枚举在编译时被替换为字面量，因此它们不会产生额外的查找开销，这在性能敏感的应用中可能会有所帮助。
* 使用场景：如果需要在运行时访问枚举成员的名称或进行枚举成员的遍历，那么应该使用普通枚举。如果优先考虑减少编译后代码的大小和提高性能，且不需要在运行时访问枚举成员的信息，则可以使用常量枚举。

## 4、.d.ts声明文件的作用

.d.ts 文件被称为声明文件，主要用于定义 TypeScript 项目中使用的 JavaScript 代码的类型信息。它们不包含具体的代码逻辑，而是提供了在 JavaScript 库、框架或者任何其他非 TypeScript 代码中使用的变量、函数、类等的类型声明。这样，TypeScript 编译器和开发工具就能提供类型检查和代码补全等功能，即使实际的代码并不是用 TypeScript 编写的。

.d.ts 文件的主要作用：

* 类型声明：为已存在的 JavaScript 代码库提供类型信息。这对于使用第三方 JavaScript 库（如 jQuery、React 等）在 TypeScript 项目中尤其重要，因为它允许 TypeScript 编译器理解这些库中对象的类型。
* 代码提示和自动补全：开发环境利用 .d.ts 文件提供的类型信息，可以为开发者提供代码提示和自动补全功能，提高开发效率。
* 编译时类型检查：虽然 TypeScript 主要用于为 JavaScript 提供类型系统，但是很多现有的 JavaScript 代码库和框架并没有直接提供 TypeScript 支持。.d.ts 文件使得开发者在使用这些库时，仍然可以享受到 TypeScript 的编译时类型检查的好处。
* 文档作用：.d.ts 文件还可以作为查看库或框架 API 的一种方式，为开发者提供了一个方便的参考手册。



如何获取 .d.ts 文件？

* 从 DefinitelyTyped 获取：[DefinitelyTyped](http://definitelytyped.org/) 是一个巨大的社区驱动的 .d.ts 文件仓库，几乎为所有流行的 JavaScript 库提供了 TypeScript 类型声明文件。通过 npm 安装相应的 @types 包（如 @types/react）即可。
* 自动生成：某些工具和库能够根据 JavaScript 代码自动生成 .d.ts 文件。
* 手动编写：对于一些特定的情况或者尚未在 DefinitelyTyped 中提供类型声明的库，开发者可以手动编写 .d.ts 文件来定义外部库或模块的类型。

## 5、TypeScript中的类型守卫是什么

类型守卫：是一种用于在运行时检查变量类型的技术。收窄变量类型范围，以确保正确推断类型。

类型守卫主要通过以下几种方式实现：

1） typeof 守卫

这是最常见的类型守卫形式，它利用 JavaScript 的 typeof 操作符来检查变量的数据类型。TypeScript 会识别这种形式的类型检查，并在这个检查的作用域内正确地推断变量类型。

```typescript
function padLeft(value: string, padding: string | number) {
    if (typeof padding === 'number') {
        return Array(paddding + 1).join(' ') + value;
    }
    if (typeof padding === 'string') {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}
```

2） instanceof 守卫

instanceof 守卫允许你检查一个对象是否为某个类的实例。这对于确定对象的具体子类型非常有用。

```typescript
class Bird {
    fly() {
        console.log('bird fly');
    }
}

class Fish {
    swim() {
        console.log('fish swim');
    }
}

function move(animal: Bird | Fish) {
    if (animal instanceof Bird) {
        animal.fly();
    } else {
        animal.swim();
    }
}
```

3） 自定义类型守卫

自定义类型守卫允许你定义一个函数来检查类型，这个函数返回一个类型谓词（Type Predicate）。类型谓词具有 “parameterName is Type” 的形式，这里的 parameterName 必须是当前函数参数的一个名称。

```typescript
fuction isFish(pet: Fish | Bird): pet is Fish {
    return (pet as Fish).swim() !== undefined;
}

// 使用自定义类型守卫
function move(pet: Fish | Bird) {
    if(isFish(pet)) {
        pet.swim();
    } else {
        pet.fly();
    }
}
```

4） 可辨识联合（Discriminated Unions）

可辨识联合是 TypeScript 特有的一种模式，它结合了联合类型、类型守卫和类型别名，用于创建一个共同的可辨识特征作为类型守卫。

```typescript
interface Circle {
    kind: 'circle';
    radius: number;
}

interface Square {
    kind: 'square';
    sideLength: number;
}

type Shape = Circle | Square;

function getArea(shape: Shape) {
    if (shape.kind === 'circle') {
        return Math.PI * shape.radius ** 2;
    } else {
        return shape.sideLength ** 2;
    }
}
```

## 6、超类型

在编程语言和类型系统中，超类型（Supertype）用于描述类型的关系，其中一个类型是另一个类型的泛化（或父类型）。这种关系通常出现在面向对象编程中的类继承或接口实现中。当一个类型 A 是另一个类型 B 的超类型时，B 被认为是 A 的子类型（Subtype）。这种类型关系支持多态性，即允许在程序中将子类型的对象当作其超类型的对象来处理。

### （1）超类型的特征

1）泛化关系：超类型通常包含更一般、更广泛的属性或方法，而子类型则在这基础上扩展或特化。

2）替代性：根据里氏替换原则（Liskov Substitution Principle, LSP），在程序中，子类型的对象应该能够替换其超类型的对象，而不影响程序的正确性。这意味着超类型定义了一组可在其所有子类型中通用的接口。

3）多态：超类型和子类型的关系允许多态性的实现，多态性是面向对象程序设计的一个核心概念，允许同一个操作对不同类型的对象进行作用，产生不同的行为。

### （2）示例

假设有一个超类型 Vehicle 和两个子类型 Car 和 Bicycle。Vehicle 定义了所有交通工具共有的属性和方法，如移动（move）。Car 和 Bicycle 分别以自己的方式实现这些方法。

```typescript
class Vehicle {
    move() {
        console.log('Moving');
    }
}

class Car extends Vehicle {
    move() {
        console.log('Driving');
    }
}

class Bicycle extends Vehicle {
    move() {
        console.log('Cycling');
    }
}
```

在这个例子中，Vehicle 是一个超类型，它是 Car 和 Bicycle 的泛化。Car 和 Bicycle 都是 Vehicle 的子类型。

### （3）在不同语境中的超类型

1）面向对象编程：超类型通常与类继承和接口实现相关。

2）类型系统：在静态类型语言中，超类型可以用来定义类型的层次结构，以支持类型的安全转换和多态性。

3）泛型编程：在泛型编程中，超类型可以用于定义泛型约束，这样泛型类型就可以接受一组具有共同超类型的类型作为参数。

## 7、ts的协变、逆变、双变和抗变是什么

在 TypeScript 中，协变（Covariance）、逆变（Contravariance）、双变（Bivariance）和 抗变（Invariant）是与类型兼容性和赋值规则相关的术语。它们描述了在类型系统中，类型如何相互关联，特别是在泛型、函数参数、返回类型等场景中的行为。

### （0）先来个总结

| <p><br></p> | 协变            | 逆变                              | 双变             | 抗变             |
| ----------- | ------------- | ------------------------------- | -------------- | -------------- |
| 赋值关系        | 子类型 -赋值-> 父类型 | 子类型 <-赋值- 父类型                   | 子类型 <-赋值-> 父类型 | 子类型 -不能赋值- 父类型 |
| 应用场景        | 数组、返回类型、只读类型  | 函数参数（—strictFunctionTypes严格模式下） | 函数参数（默认）       | 无继承关系          |

便于理解的个人总结：子类型比父类型更有能力，对于继承关系，子类型比父类型拥有更多属性和方法，对于函数，子类型比父类型接受更宽容更泛化的参数，所以处理能力更强、更通用

### （1）协变

【定义】如果类型 A 是类型 B 的子类型，则由 A 构成的复合类型（如数组、Promise等）也是由 B 构成的相应复合类型的子类型。

协变是最直观的类型关系，它表示子类型可以被赋值给父类型。这种关系通常适用于数组、返回类型、只读类型等。

【举例】如果 Dog 是 Animal 的子类型，那么 Dog\[] 是 Animal\[] 的子类型。

### （2）逆变

【定义】如果类型 A 是类型 B 的子类型，那么 B 作为函数参数类型是 A 作为相应函数参数类型的子类型。这意味着函数参数的类型关系与返回类型相反。

逆变表示父类型可以被赋值给子类型，这种关系通常适用于函数参数。在 TypeScript 中，默认情况下函数参数是双变的，但在开启 --strictFunctionTypes 的严格模式下，函数参数会表现为逆变。

【举例】如果有函数类型 FunctionA: (base: Base) => Base 和 FunctionB: (derived: Derived) => Derived，并且 Derived 是 Base 的子类型，在逆变的情况下，入参更宽容的FunctionA是入参更具体的FunctionB的子类型，按照逆变父类型可以赋值给子类型的赋值规则，FunctionB 能赋值给期望 FunctionA 类型的变量。



**逆变理解起来有些困难，我们来做进一步理解。**

【更进一步理解 1】假设有两个类 A 和 B，其中 A 是 B 的子类。按照直觉，我们可能会认为 A 的实例可以用在期望 B 的地方，因为子类实例通常拥有父类的所有属性和方法（这是协变）。然而，当这个概念应用到函数参数时，情况就有所不同了。因为逆变关注的是函数类型的兼容性。根据逆变的定义，如果我们将类型 B 视为函数参数的一个“更泛化的类型”，而 A 为“更具体的类型”，那么在函数参数的上下文中，“能够接受更泛化类型 B 的函数”被认为是“能够接受更具体类型 A 的函数”的子类型。

逆变的逻辑解释

* 正向理解：一个函数能接受“更泛化”的类型作为参数意味着它更“宽容”或“灵活”。因为它能处理的情况更多，不仅仅局限于具体的子类型。
* 逆向逻辑：如果你有一个函数 f1，它的参数类型是父类型 B，这意味着它可以接受 B 以及 B 的所有子类型（如 A）作为参数。现在，如果有另一个函数 f2，它的参数类型是子类型 A。按照逆变的逻辑，f1 能被认为是 f2 的子类型，因为 f1 在参数接受上比 f2 更加宽容或通用。

结论：逆变描述了一种类型系统中的关系，其中“更宽容的函数（可以接受父类型参数）”在类型系统中被认为是“更严格的函数（仅接受子类型参数）”的子类型。

【进一步理解 2：为什么子类实例拥有父类实例所有属性和方法，子类就能赋值给父类？】

这个问题涉及到面向对象编程中的继承和多态的概念，以及类型系统中的协变性。

1）继承和多态

在面向对象编程中，当一个类（子类）继承另一个类（父类）时，子类自动获得父类的所有属性和方法。这意味着从父类派生出的任何子类实例，在逻辑上都是父类的一个实例。这种关系允许子类对象在需要父类对象的地方被使用，这就是多态。

2）协变性

协变性是类型系统中的一个概念，它描述了如何基于类型之间的继承关系来保持类型兼容性。在具有协变性的类型系统中，如果类型 A 是类型 B 的子类型，那么由 A 类型的对象集合也是由 B 类型的对象集合的子类型。这同样适用于单个对象：如果 Dog 是 Animal 的子类型，那么 Dog 类型的对象也可以被视为 Animal 类型的对象。



那么为什么子类可以赋值给父类？

1）里氏替换原则（Liskov Substitution Principle, LSP）：这是面向对象设计原则中的一个重要概念，由 Barbara Liskov 提出。它指出，如果 S 是 T 的子类型，那么类型为 T 的对象可以在程序中被类型为 S 的对象替换，而不改变程序的期望属性（如正确性）。这意味着子类对象可以在需要父类对象的任何地方使用，而不会影响程序逻辑。

2）类型兼容性：在继承关系中，子类扩展了父类的功能，但保留了父类的行为。因此，从类型的角度看，子类类型的对象是与父类类型兼容的。在需要父类类型的对象的场景中使用子类类型的对象不会引起问题，因为子类对象“至少”拥有父类对象的所有属性和方法。

3）安全性：由于子类对象包含了父类的所有属性和方法，使用子类对象代替父类对象不会导致运行时错误。相反，如果尝试将父类对象赋值给子类类型的变量，就可能存在问题，因为父类对象可能不包含子类特有的属性和方法。



总结来说，子类实例能够赋值给父类，是因为子类继承了父类的所有属性和方法，并可能添加了一些新的属性和方法。这种从子类到父类的赋值关系是安全的，因为子类实例在逻辑上也是一个父类实例，满足了面向对象设计的多态性和类型系统的协变性。

### （3）双变

【定义】如果类型系统允许你将类型 A 的值赋给类型 B，反之亦然，即使 A 和 B 之间存在子类型关系，这种情况被称为双变。在 TypeScript 中，函数参数类型默认是双变的。

【举例】即使 Dog 是 Animal 的子类型，在双变的情况下，接受 Animal 类型参数的函数可以被赋值给期望接受 Dog 类型参数的函数变量，反之亦然。

### （4）抗变

【定义】仅当类型完全相同时，才允许相互赋值。如果类型 A 既不是类型 B 的子类型也不是其超类型，那么这两个类型就是抗变的。

【举例】在抗变的情况下，如果有类型 A 和类型 B，并且它们之间没有继承关系，那么类型为 A 的变量就不能赋值给类型为 B 的变量，反之亦然。

### （5）在 TypeScript 中的应用

【函数参数】在 TypeScript 中，函数参数类型默认是双变的，但这可能会导致一些意外行为。TypeScript 从 2.6 版本开始引入了 --strictFunctionTypes 标志，使得函数参数类型在严格模式下表现为逆变，增强了类型安全性。

【泛型】TypeScript 中的泛型和类型别名通常表现为协变，但在某些特定情况下（如使用 readonly 修饰符或特定的类型操作符时）可能会有不同的变性行为。

## 8、Typescript的Mixins如何使用

在 TypeScript（和 JavaScript） 中不支持直接多重继承，而是通过Mixins来实现多重继承，Mixins通过组合而非继承的方式来共享多个类的方法和属性。

### （1）使用示例

1）首先，定义两个 Mixins，每个 Mixin 都有不同的属性和方法：

<pre class="language-typescript"><code class="lang-typescript"><strong>// Disposable Mixin
</strong>class Disposable {
  isDisposed: boolean;
  dispose() {
    this.isDisposed = true;
  }
}

// Activatable Mixin
class Activatable {
  isActive: boolean;
  activate() {
    this.isActive = true;
  }
  deactivate() {
    this.isActive = false;
  }
}
</code></pre>

2）然后，定义一个帮助函数来应用这些 Mixins：

```typescript
function applyMixins(derivedCtor: any, baseCtors: any[]) {
  baseCtors.forEach(baseCtor => {
    Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
      derivedCtor.prototype[name] = baseCtor.prototype[name];
    });
  });
}
```

3）最后，创建一个类并将这些 Mixins 应用到该类上：

```typescript
class SmartObject implements Disposable, Activatable {
  constructor() {
    setInterval(() => console.log(this.isActive + ':' + this.isDisposed), 500);
  }

  interact() {
    this.activate();
  }

  // Disposable
  isDisposed: boolean = false;
  dispose: () => void;
  // Activatable
  isActive: boolean = false;
  activate: () => void;
  deactivate: () => void;
}

applyMixins(SmartObject, [Disposable, Activatable]);

let smartObj = new SmartObject();
setTimeout(() => smartObj.interact(), 1000);
```

在这个例子中，SmartObject 类通过 applyMixins 函数集成了 Disposable 和 Activatable 两个 Mixins 的功能。这使得 SmartObject 实例可以调用这两个 Mixins 中定义的所有方法。

### （2）注意事项

使用 Mixins 时要注意，Mixins 可能会引起命名冲突和复杂的继承链问题，需要谨慎设计 Mixins 以避免这些问题。此外，TypeScript 的 Mixins 实现依赖于 JavaScript 的原型继承和对象属性复制，这可能会影响到性能和类型系统的清晰度。因此，在使用 Mixins 之前，务必考虑是否有更简单、更清晰的方法来实现所需的功能复用。

## 9、Typescript装饰器执行顺序

ts装饰器类型有：参数装饰器、属性装饰器、方法装饰器、访问器装饰器、类装饰器。

执行顺序：内层先执行，同一层级的按书写顺序从上到下、从左到右；但是类装饰器是从下到上。如果有装饰器工厂函数，则装饰器工厂函数最先执行。

注意：

* 如果没有参数装饰器，则顺序为：属性装饰器、方法或访问器装饰器、类装饰器；
* 如果被方法装饰器装饰的方法的参数也有参数装饰器，则顺序为：参数装饰器、方法或访问器装饰器、属性装饰器、类装饰器。

## 10、已有一个接口，想在这个接口之上加一个属性，怎么做

为现有接口添加新属性，可以通过2种方式来实现：

（1）通过extends扩展接口：创建一个新接口，扩展现有的接口并添加新属性。这是推荐做法，因为这种方式的代码更具可读性和可维护性。

```typescript
// 定义原始接口
interface OriginalInterface {
    property1: string;
    property2: number;
}

// 扩展接口并添加新属性
interface ExtendedInterface extends OriginalInterface {
    newProperty: boolean;
}

// 使用扩展接口
const example: ExtendedInterface = {
    property1: 'example',
    property2: 123,
    property3: true,
}
```

（2）类型声明合并：再次声明该已有接口，并添加新属性。某些场景下这种方式也很有用，比如当需要为第三方库中的接口添加属性时。

```typescript
// 定义原始接口
interface OriginalInterface {
    property1: string;
    property2: number;
}

// 在同一模块或不同模块中再次声明接口，并添加新属性
interface OriginalInterface {
    newProperty: boolean;
}

// 使用扩展接口
const example: ExtendedInterface = {
    property1: 'example',
    property2: 123,
    property3: true,
}
```

## 11、一个接口有3个属性a、b、c，现在想定义一个类型，只使用其中的a、b属性，该如何实现？

使用Pick实用类型，从现有接口中选择特定的属性来创建一个新的类型。

```typescript
interface OriginalInterface {
    a: number;
    b: string;
    c: boolean;
}

// 使用 Pick 从 OriginalInterface 中选择 a 和 b 属性
type ABType = Pick<OriginalInterface, 'a' | 'b'>;

// 示例使用
const example: ABType = {
    a: 1,
    b: 'example',
    // c: true // 错误：类型 "ABType" 中不包含属性 "c"
}
```

## 12、可选和必选之间如何做转换？比如一个接口，有a、b、c，我想把这个接口复制出来，把它的c变成可选的，再复制一份出来，把c变成不可选

可以使用 Partial 和 Required 类型操作符来分别将属性变为可选和必选。举例：

```typescript
// 原始接口
interface MyInterface {
    a: string;
    b: number;
    c: boolean;
}

// 将 c 变为可选
type MyInterfaceWithOptionalC = Omit<MyInterface, 'c'> & Partial<Pick<MyInterface, 'c'>>;

// 将 c 变为必选
type MyInterfaceWithRequiredC = Omit<MyInterface, 'c'> & Required<Pick<MyInterface, 'c'>>;

// 示例使用
const example1: MyInterfaceWithOptionalC = {
    a: 'example',
    b: 123,
    // c可以不存在
}

const example2: MyInterfaceWithOptionalC = {
    a: 'example',
    b: 123,
    c: true, // c也可以存在
}

const example3: MyInterfaceWithRequiredC = {
    a: 'example',
    b: 123,
    c: true, // c必须存在
}
```

其中：\
1）`Omit<MyInterface, 'c'>`

创建一个新类型，去除 MyInterface 中的 c 属性。

2）`Partial<Pick<MyInterface, 'c'>>`

创建一个新类型，包含 MyInterface 中的 c 属性，并将其设为可选。

3）`Required<Pick<MyInterface, 'c'>>`

创建一个新类型，包含 MyInterface 中的 c 属性，并将其设为必选。

## 13、Partial

Partial使某个类型的所有属性变为可选属性。举例说明：

1）简单的 Partial 用法

```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

// 使用 Partial 将所有属性变为可选
type PartialUser = Partial<User>;

// 示例使用
const user1: PartialUser = {};
const user2: PartialUser = { name: 'Alice' };
const user3: PartialUser = { name: 'Bob', age: 30 }

console.log(user1); // 输出：{}
console.log(user2); // 输出：{ name: 'Alice' }
console.log(user3); // 输出：{ name: 'Bob', age: 30 }
```

2）在函数中使用 Partial

我们可以在函数参数中使用 Partial，使得函数能够接受部分更新参数。

```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

function updateUser(user: User, updates: Partial<User>): User {
    return {...user, ...updates };
}

// 示例使用
const user: User = { name: 'Charlie', age: 25, email: 'charlie@example.com' };

const updateUser = updateUser(user, { age: 35 });
console.log(updateUser); // 输出：{ name: 'Charlie', age: 35, email: 'charlie@example.com' }
```

3）Partial 和接口组合

可以通过 Partial 将某些接口属性变为可选，然后将其与其他接口组合使用。

比如下面定义了一个新接口 UpdateUserRequest，它继承自 Partial\<User>，所以拥有3个可选属性，并添加了一个新的必选属性 userId。

```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

interface UpdateUserRequest extends Partial<User> {
    userId: number;
}

// 示例使用
const updateRequest: UpdateUserRequest = {
    userId: 1,
    email: 'newemail@example.com'
}

console.log(updateRequest); // 输出：{ userId: 1, email: 'newemail@example.com' }
```

## 14、举个泛型的例子，比如一个方法参数是泛型，你是如何使用的

Typescript泛型允许创建可重用的组件，能够处理多种类型而不是单一的类型。泛型广泛应用于函数、接口和类中。

1）泛型函数

以下泛型函数可以处理不同类型的数据，并返回相同类型的数据。

```typescript
// 定义一个泛型函数，参数和返回值类型相同
function identity<T>(arg: T): T {
    return arg;
}

// 使用泛型函数
const stringOutput = identity<string>('hello');
const numberOutput = identity<number>(42);

console.log(stringOutput); // 输出：'hello'
console.log(numberOutput); // 输出：42
```

2）泛型接口

可以使用泛型定义接口，以便灵活地处理不同类型的数据。

下面例子中，Container 接口接受一个泛型参数 T，使得 value 属性可以是任何类型。

```typescript
// 定义一个泛型接口
interface Container<T> {
    value: T;
}

// 使用泛型接口
const stringContainer: Container<string> = { value: 'hello' };
const numberContainer: Container<number> = { value: 42 };

console.log(stringContainer); // 输出：'hello'
console.log(numberContainer); // 输出：42
```

3）泛型类

```typescript
// 定义一个泛型类
class Box<T> {
    contents: T;
    
    constructor(contents: T) {
        this.contents = contents;
    }
    
    getContents(): T {
        return this.contents;
    }
}

// 使用泛型类
const stringBox = new Box<string>('A string');
const numberBox = new Box<number>(123);

console.log(stringBox.getContents()); // 输出：'A string'
console.log(numberBox.getContents()); // 输出：123
```

4）泛型约束

有时我们希望泛型参数满足某些条件，可以使用泛型约束。比如下面的例子里，使用 `T extends Lengthwise` 约束了泛型参数 T，确保 T 类型具有 length 属性。

```typescript
// 定义一个具有约束的泛型接口
interface Lengthwise {
    length: number;
}

// 定义一个泛型函数，要求 T 类型必须具有 length 属性
function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length); // 现在可以访问 length 属性
    return arg;
}

// 使用泛型函数
const arrayOutput = loggingIdentity([1, 2, 3]); // 输出：3
const stringOutput = loggingIdentity('hello'); // 输出：5

// 编译错误: Argument of type 'number' is not assignable to parameter of type 'Lengthwise'
// const numberOutput = loggingIdentity(42);
```

5）多个泛型参数

可以定义接受多个泛型参数的函数。

```typescript
// 定义一个接受两个泛型参数的函数
function combine<T, U>(first: T, second: U): [T, U] {
    return [first, second];
}

// 使用泛型函数
const combineResult = combine<string, number>('hello', 42);
console.log(combineResult); // 输出：['hello', 42]
```

## 15、Record类型

Record类型是Typescript中一个实用的泛型类型，提供了一种简介的方法来定义具有指定类型键和值的对象。

### （1）Record类型定义

```typescript
type Record<K extends keyof any, T> = {
    [P in K]: T;
}
```

其中K表示键的类型，必须是一个字符串或数字；T表示值的类型。

### （2）作用和用途

1）定义具有特定键和值类型的对象

使用 Record 可以快速定义一个对象类型，其所有键的类型和所有值的类型都是已知的，例如：

```typescript
// 定义一个对象类型，键是字符串类型，值是数字类型
type NumberDictionary = Record<string, number>;

const scores: NumberDictionary = {
    Alice: 95,
    Bob: 82,
    Charlie: 88
}
```

2）限制对象的键和值类型

Record 允许你限制对象的键和值的类型，使得对象更加类型安全，例如：

```typescript
interface Person {
    name: string;
    age: number;
}

// 定义一个对象类型，键是字符串类型，值是 Person 类型
type PersonDictionary = Record<string, Person>;

const people: PersonDictionary = {
    person1: { name: 'Alice', age: 25 },
    person2: { name: 'Bob', age: 35 },
}
```

3）构建复杂的对象类型

通过 Record 可以构建更复杂的对象类型，特别是当对象的键和值有复杂的类型要求时。例如：

```typescript
interface User {
    id: number;
    name: string;
}

type UsersById = Record<number, User>;

const users: UsersById = {
    1: { id: 1, name: 'Alice' },
    2: { id: 2, name: 'Bob' },
}
```
