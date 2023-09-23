# Extension methods guidelines

Extension methods are a language feature that allows static methods to be called using instance method call syntax. These methods must take at least one parameter, which represents the instance the method is to operate on.

The class that defines such extension methods is referred to as the "sponsor" class, and it must be declared as static. To use extension methods, one must import the namespace defining the sponsor class.

❌ AVOID frivolously defining extension methods, especially on types you don't own.

If you do own source code of a type, consider using regular instance methods instead. If you don't own, and you want to add a method, be very careful. Liberal use of extension methods has the potential of cluttering APIs of types that were not designed to have these methods.

✔️ CONSIDER using extension methods in any of the following scenarios:

To provide helper functionality relevant to every implementation of an interface, if said functionality can be written in terms of the core interface. This is because concrete implementations cannot otherwise be assigned to interfaces. For example, the LINQ to Objects operators are implemented as extension methods for all IEnumerable<T> types. Thus, any IEnumerable<> implementation is automatically LINQ-enabled.

When an instance method would introduce a dependency on some type, but such a dependency would break dependency management rules. For example, a dependency from String to System.Uri is probably not desirable, and so String.ToUri() instance method returning System.Uri would be the wrong design from a dependency management perspective. A static extension method Uri.ToUri(this string str) returning System.Uri would be a much better design.

❌ AVOID defining extension methods on System.Object.

VB users will not be able to call such methods on object references using the extension method syntax. VB does not support calling such methods because, in VB, declaring a reference as Object forces all method invocations on it to be late bound (actual member called is determined at run time), while bindings to extension methods are determined at compile-time (early bound).

Note that the guideline applies to other languages where the same binding behavior is present, or where extension methods are not supported.

❌ DO NOT put extension methods in the same namespace as the extended type unless it is for adding methods to interfaces or for dependency management.

❌ AVOID defining two or more extension methods with the same signature, even if they reside in different namespaces.

✔️ CONSIDER defining extension methods in the same namespace as the extended type if the type is an interface and if the extension methods are meant to be used in most or all cases.

❌ DO NOT define extension methods implementing a feature in namespaces normally associated with other features. Instead, define them in the namespace associated with the feature they belong to.

❌ AVOID generic naming of namespaces dedicated to extension methods (e.g., "Extensions"). Use a descriptive name (e.g., "Routing") instead.

## Links

* [↑ Extension Methods (Member Design Guidelines)](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/extension-methods)
