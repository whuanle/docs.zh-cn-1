---
description: readonly 关键字 - C# 参考
title: readonly 关键字 - C# 参考
ms.date: 04/14/2020
f1_keywords:
- readonly_CSharpKeyword
- readonly
helpviewer_keywords:
- readonly keyword [C#]
ms.assetid: 2f8081f6-0de2-4903-898d-99696c48d2f4
ms.openlocfilehash: b1bab5af18216fcef2162179493dbbb59e3470cf
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89137170"
---
# <a name="readonly-c-reference"></a>readonly（C# 参考）

`readonly` 关键字是一个可在四个上下文中使用的修饰符：

- 在[字段声明](#readonly-field-example)中，`readonly` 指示只能在声明期间或在同一个类的构造函数中向字段赋值。 可以在字段声明和构造函数中多次分配和重新分配只读字段。
  
  构造函数退出后，不能分配 `readonly` 字段。 此规则对于值类型和引用类型具有不同的含义：
  
  - 由于值类型直接包含数据，因此属于 `readonly` 值类型的字段不可变。
  - 由于引用类型包含对其数据的引用，因此属于 `readonly` 引用类型的字段必须始终引用同一对象。 该对象是可变的。 `readonly` 修饰符可防止字段替换为引用类型的其他实例。 但是，修饰符不会阻止通过只读字段修改字段的实例数据。

  > [!WARNING]
  > 包含属于可变引用类型的外部可见只读字段的外部可见类型可能存在安全漏洞，可能会触发警告 [CA2104](/visualstudio/code-quality/ca2104)：“不要声明只读可变引用类型。”

- 在 `readonly struct` 类型定义中，`readonly` 指示结构类型是不可变的。 有关详细信息，请参阅[结构类型](../builtin-types/struct.md)一文中的 [`readonly` 结构](../builtin-types/struct.md#readonly-struct)一节。
- 在结构类型内的实例成员声明中，`readonly` 指示实例成员不修改结构的状态。 有关详细信息，请参阅[结构类型](../builtin-types/struct.md)一文中的 [`readonly` 实例成员](../builtin-types/struct.md#readonly-instance-members)部分。
- 在 [`ref readonly` 方法返回](#ref-readonly-return-example)中，`readonly` 修饰符指示该方法返回一个引用，且不允许向该引用写入内容。

在 C# 7.2 中添加了 `readonly struct` 和 `ref readonly` 上下文。 在 C# 8.0 中添加了 `readonly` 结构成员

## <a name="readonly-field-example"></a>Readonly 字段示例

在此示例中，只能在类构造函数中给字段 `year` 赋初始值，在方法 `ChangeYear` 中无法更改其值：

[!code-csharp[Readonly Field example](snippets/ReadonlyKeywordExamples.cs#ReadonlyField)]

只能在下列上下文中对 `readonly` 字段进行赋值：

- 在声明中初始化变量时，例如：

  ```csharp
  public readonly int y = 5;
  ```

- 在包含实例字段声明的类的实例构造函数中。
- 在包含静态字段声明的类的静态构造函数中。

只有在这些构造函数上下文中，将 `readonly` 字段作为 [out](out-parameter-modifier.md) 或 [ref](ref.md) 参数传递才有效。

> [!NOTE]
> `readonly` 关键字不同于 [const](const.md) 关键字。 `const` 字段只能在该字段的声明中初始化。 可以在字段声明和任何构造函数中多次分配 `readonly` 字段。 因此，根据所使用的构造函数，`readonly` 字段可能具有不同的值。 另外，虽然 `const` 字段是编译时常量，但 `readonly` 字段可用于运行时常量，如下面的示例所示：
>
> ```csharp
> public static readonly uint timeStamp = (uint)DateTime.Now.Ticks;
> ```

[!code-csharp[Initialize readonly Field example](snippets/ReadonlyKeywordExamples.cs#InitReadonlyField)]

在前面的示例中，如果使用类似以下示例的语句：

```csharp
p2.y = 66;        // Error
```

你将收到编译器错误消息：

**无法对只读的字段赋值（构造函数或变量初始值指定项中除外）**

## <a name="ref-readonly-return-example"></a>Ref readonly 返回示例

`ref return` 上的 `readonly` 修饰符指示返回的引用无法修改。 下面的示例返回了一个对来源的引用。 它使用 `readonly` 修饰符来指示调用方无法修改来源：

[!code-csharp[readonly return example](snippets/ReadonlyKeywordExamples.cs#ReadonlyReturn)]

所返回的类型不需要为 `readonly struct`。 `ref` 能返回的任何类型都能由 `ref readonly` 返回。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

你还可查看语言规范建议：

- [readonly ref 和 readonly 结构](~/_csharplang/proposals/csharp-7.2/readonly-ref.md)
- [readonly 结构成员](~/_csharplang/proposals/csharp-8.0/readonly-instance-members.md)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [修饰符](index.md)
- [const](const.md)
- [字段](../../programming-guide/classes-and-structs/fields.md)
