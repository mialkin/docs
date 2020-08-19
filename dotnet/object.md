# Object

## Methods

### Object.MemberwiseClone

The `MemberwiseClone` method creates a shallow copy by creating a new object, and then copying the nonstatic fields of the current object to the new object. If a field is a value type, a bit-by-bit copy of the field is performed. If a field is a reference type, the reference is copied but the referred object is not; therefore, the original object and its clone refer to the same object.

```csharp
protected object MemberwiseClone ();
```

```output
7, hello, 10
7, hello, 10
----------------
8, bye, 11
7, hello, 11
```
