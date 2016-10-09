# Introduction to Dependency Injection Container

The Spring4D Dependency Injection Container allows you to register classes as well as interfaces that are implemented by given classes.

The Dependency Injection container allows you to register classes and classes as implementing interfaces. This enables you to later, at the composition root of your application, retrieve created instances of those registered classes. 

Here is a simple example.  Consider the following code:

```
#!delphi
Container.RegisterType<TMyClass>.Implements<IMyClass>
```

In this case, the above code basically means "Register the type `TMyClass` as implementing the `IMyClass` interface". 

This has the benefit of allowing you to decouple the creation of a given class from its use as an interface.  Note that you can create and use an instance of `TMyClass` without having to reference the unit in which it resides. 

## Registering by Name

The container allows you to register types by name.  Sometimes you have more than one implementation for a given interface, and you can register both of those types against the same class using a `string` name.  For instance:

```
#!delphi
Container.RegisterType<TFirstClass>.Implements<ISomeIntf>('First');
Container.RegisterType<TSecondClass>.Implements<ISomeIntf>('Second');
``` 

## Constructor Injection

The key to Dependency Injection -- and thus the use of the DI Container -- is constructor injection.  Constructor injection is the notion that your classes should ask for what they need and not create what they need.  In order to "ask" for something, they should declare a parameter on the constructor for the type they need.   

For example, here is a constructor from a class that creates what it needs instead of asking for what it needs:

```
#!delphi
constructor TSprocketManager.Create;
begin
  FSprocket :=  TSprocket.Create
end;
```

This class is tightly coupled to the `TSprocket` class.  Instead, the class should ask for what it needs:

```
#!delphi
constructor TSprocketManager.Create(aSprocket: ISprocket);
begin
  FSprocket := aSprocket;
end;
```

Notice that here, the constructor makes an open, clear declaration:  "In order to use me, you need to pass me an `ISprocket` instance, otherwise, I won't even work".
