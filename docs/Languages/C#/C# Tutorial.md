# C# Tutorial

How to start?
[C# Guide](https://docs.microsoft.com/zh-tw/dotnet/csharp/)

## Value Types and Reference Types

### Value Type

There are two kinds of value types:

- Structs
- Enumerations

The *simple types* are a set of predefined struct types provided by C# and comprise the following types:

- Integral types: integer numeric types and the char type
- Floating-point types
- bool

### Reference Type

The following keywords are used to declare reference types:

- class
- interface
- delegate

C# also provides the following built-in reference types:

- dynamic
- object
- string

### Passing an argument by reference

When used in a method's parameter list, the `ref` keyword indicates that an argument is passed by reference, not by value. The effect of passing by reference is that any change to the argument in the called method is reflected in the calling method. For example, if the caller passes a local variable expression or an array element access expression, and the called method replaces the object to which the ref parameter refers, then the caller’s local variable or the array element now refers to the new object when the method returns.

The `out` keyword causes arguments to be passed by reference. It is like the `ref` keyword, except that `ref` requires that the variable be initialized before it is passed.

## Garbage Collection

.NET's garbage collector manages the allocation and release of memory for your application. Each time you create a new object, the common language runtime allocates memory for the object from the managed heap. As long as address space is available in the managed heap, the runtime continues to allocate space for new objects. However, memory is not infinite. Eventually the garbage collector must perform a collection in order to free some memory. The garbage collector's optimizing engine determines the best time to perform a collection, based upon the allocations being made. When the garbage collector performs a collection, it checks for objects in the managed heap that are no longer being used by the application and performs the necessary operations to reclaim their memory.

### Boxing and Unboxing

Boxing is the process of converting a value type to the type `object` or to any interface type implemented by this value type. When the CLR boxes a value type, it wraps the value inside a System.Object and stores it on the managed heap. Unboxing extracts the value type from the object. Boxing is implicit; unboxing is explicit. The concept of boxing and unboxing underlies the C# unified view of the type system in which a value of any type can be treated as an object.

#### Performance

It is best to avoid using value types in situations where they must be boxed a high number of times, for example in non-generic collections classes such as System.Collections.ArrayList. You can avoid boxing of value types by using generic collections such as System.Collections.Generic.List<T>. Boxing and unboxing are computationally expensive processes. When a value type is boxed, an entirely new object must be created. This can take up to 20 times longer than a simple reference assignment. When unboxing, the casting process can take four times as long as an assignment. For more information, see Boxing and Unboxing.

### Finalizer

Finalizers (which are also called destructors) are used to perform any necessary final clean-up when a class instance is being collected by the garbage collector.

### Disposable Objects

In .NET, objects that reference unmanaged resources implement the IDisposable interface. When you're done using the object, you call the object's Dispose() method, which is responsible for releasing any unmanaged resources. .NET languages provide a convenient using syntax for such objects.

## Delegates and Lambdas

A delegate is represented by a method signature. Any method with that signature can be assigned to the delegate and is executed when the delegate is invoked.

Delegates are like C++ function pointers except that they're type safe. They're a kind of disconnected method within the CLR type system. Regular methods are attached to a class and are only directly callable through static or instance calling conventions.

In .NET, delegates are commonly used in event handlers, in defining asynchronous operations, and in lambda expressions, which are a cornerstone of LINQ.

### Event

Events are a special kind of multicast delegate that can only be invoked from within the class or struct where they are declared (the publisher class). If other classes or structs subscribe to the event, their event handler methods will be called when the publisher class raises the event.

## Generics

Generics allow the programmer to introduce a type parameter when designing their classes that allows the client code (the users of the type) to specify the exact type to use in place of the type parameter.

Generics were added to help programmers implement generic data structures. Before their arrival in order for a type such as the List type to be generic, it would have to work with elements that were of type object. This had various performance and semantic problems, along with possible subtle runtime errors. The most notorious of the latter is when a data structure contains, for instance, both integers and strings, and an InvalidCastException is thrown on working with the list's members.

## Other

### Closures

### IEnumerable, yield

### Language Integrated Query (LINQ)

### Thread/Task

## Library

### Nuget

### NLog

### Unit test

### Dapper
