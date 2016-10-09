# Using Delphi and C++ combined with Spring4d

This article describes how to use RAD Studio to write applications in Delphi and C++ combined and use Spring container to inject implementations written in either language into field, properties etc., so that the user of the interface doesn't need to care which language was used to write the implementation.

## Motivation
Many programmers don't like or don't want to learn Delphi (especially hard core C programmers) so in order to be able to reuse code it would be very helpful to be able to use implementations written in C++/C.

In a company that has multiple development teams it is next to impossible to make them all use Delphi. This is even more true if some of those teams are making microcontroller code. Using the technique described below, you should be able to build an application that uses C++ implementation of some of the interfaces or use wrappers for "low-level" C code if needed.

## Failed approaches

Let me first talk about some approaches that may look simpler to use but that doesn't lead to working solution.

### Single project

One of the things you think about first is: "Hey C++ Builder project can use Delphi code directly right?". It is true for simple applications but more complex units (like Spring) cannot be translated to HPP (which you need to be able to call Delphi code) so you cannot use the `TContainer` directly. If you're careful enough with your units being specified only in `implmentation` section, your code might work but in case of Spring, the compiler will complain and produce a bunch of internal or linker errors.

### C++ BPL

Since you want to think about your Delphi application as primary, it sounds logical creating a C++ BPL and then linking it with Delphi (either statically or dynamically). This is not possible because C++ package compilation won't generate DCP file that could be used to add the BPL as runtime package.

## Working solution

Since none of the above methods worked, the last solution I didn't try was using **main application written in C++ that use Delphi written package**. C++ project need BPI file instead of DCP file to add the package as runtime package (which gets generated for both Delphi and C++ packages).

As I pointed out above you still cannot use Spring directly as the HPPs won't get compiled, Spring registration needs to be done using a wrapper that shadows calls to Spring registration so that Spring can be hidden in the implementations section which don't get into HPP so it can be safely included in the C++ code.

It is better to put your interface definitions into another file(s) but be careful to only include units that won't add Spring to interface section. The downside is that we cannot use Spring Collections in C++ and other spring stuff. This way interfaces can be shared more easily between Delphi and C++.

Other than that, you should be able to create Delphi and C++ implementations normally (see C++ builder help and the example below).

As said above registration needs to be handled separately for C++, Delphi registration can use the same wrapper or `TContainer` directly. But there is one more thing to not for C++, C++ doesn't support attributes so injections and lifetime need to be specified during the registration. Also you need to add RTTI to fields if you want them to be injected to (as opposed to what the help says that field RTTI should be added by default but it's not).

### Steps

These steps are for console application, I will add/modify the steps for VCL/FMX (mobile) project later.

#### Project group preparation

 1. Create new C++ (console) project (make sure to set *Target framework* to either VCL or FMX or some won't get linked in)
 1. Add Delphi package to the group
 1. Optionally set the output location for the BPL to some "known" location and HPP/LIB locations so the sources don't get mixed up with other projects
 1. ~~Make sure *Use Debug DCUs* are disabled in both projects~~ (not needed if using dynamic linking)
 1. In the C++ projects enable *Link with runtime packages*
 1. Set the BPL project as the C++ project dependency
 1. I'm not exactly sure why but sometimes you need to add the Delphi package's LIB to CBPROJ under `<PropertyGroup Condition="'$(Base)'!=''">/<AllPackageLibs>` (sometimes Delphi do that by itself sometimes don't)
 1. Now you may test that the project is working by creating a Delphi procedure and calling it from C++ just to be sure you're on the right track
 1. Add Spring sources to *Search path* of the Delphi package

#### The code

Note that there is no PAS file in the C++ project (but it could be if you want to). There are only few snippets of the code but the full source code will be available for download.

##### Services.pas

Contains just a few interfaces the we'll use in Delphi and C++ code

```delphi
  IService1 = interface
    ['{85E663E1-A795-4645-B2DF-05616183322B}']
    procedure Foo;
  end;

  IService2 = interface
    ['{A449C7B4-625C-4032-A064-6A2D6B333BF0}']
    procedure Bar;
  end;
```

##### DelphiUnit.pas

Contains a sample code that might become Delphi part of your application logic.

The `InitApp` creates and returns the container that C++ code will use to register its implementations, `RunApp` starts the application (only calls all the implementations that we registered).
`TDelphiImpl2WithCppDep` is a simple implementation that uses injected C++ implementation, note that we can use attributes normally.

```delphi
unit DelphiUnit;

interface

uses
  Spring.Container.Common;

function InitApp : IContainer;
procedure RunApp;

implementation

uses
  Spring.Container,
  Spring.Container.Common,
  Services;

var Container : TContainer;

type
  TDelphiImpl2WithCppDep = class(TInterfacedObject, IService2)
  private
    [Inject]
    FDep : IService1;
  public
    procedure Bar;
  end;

function InitApp : IContainer;
begin
  Assert(Container = nil);
  Container:=TContainer.Create();
  Result:=Container;
end;

procedure RunApp;
begin
  Container.RegisterType<TDelphiImpl2WithCppDep>;
  Container.Build;
  Container.Resolve<IService1>.Foo;
  Container.Resolve<IService2>.Bar;
  Container.Resolve<IService1>('cppdep').Foo;
end;

{ TService2WithCppDep }

procedure TDelphiImpl2WithCppDep.Bar;
begin
  Writeln('TDelphiImpl2WithCppDep.Bar');
  FDep.Foo;
end;

initialization
finalization
  Container.Free;

end.
```

##### Main.cpp

Contains a sample code that might become Delphi part of your application logic. It is also the entry point of the sample application. I will comment some parts more closely, but everything here is documented by Embarcadero help as well.
`TCppImpl1` is a simple C++ implementation of the `IService1` interface that gets called from Delphi code.
`TCppImpl1WithDelphiDep` is a C++ implementation that gets a name so we don't get container registration exception which requires `IService2` defined in Delphi code which in turn requires the original `IService1` implementation.

```cpp
#include <vcl.h>
#include <windows.h>

#pragma hdrstop
#pragma argsused

#include <tchar.h>

#include <stdio.h>

```
We're only including HPPs that don't include Spring since it wouldn't compile.
```cpp
#include "DelphiUnit.hpp"
#include "Services.hpp"

namespace CppUnit
{
  class TCppImpl1 : public TInterfacedObject, public IService1
  {
  public:
    void __fastcall Foo()
    {
      printf("TCppImpl1.Foo\n");
    }
```
This is how we add ```AddRef```, ```Release``` and ```QueryInterface``` methods implementation to a class in C++.
```cpp
  public:
    INTFOBJECT_IMPL_IUNKNOWN(TInterfacedObject);
  };
```
Note that in order to use field injections we need to alter RTTI generation.
```cpp
  #pragma push explicit_rtti
  #pragma explicit_rtti methods (__published, public) properties (__published, public) fields(__published, public, protected, private)
  class TCppImpl1WithDelphiDep : public TInterfacedObject, public IService1
  {
  private:
    IService2* m_dep;
  public:
    void __fastcall Foo()
    {
      printf("TCppImpl1WithDelphiDep.Foo\n");
      m_dep->Bar();
    }
```
We need to define destructor that releases the object since C++ doesn't do reference counting by default. `DelphiInterface<T>` adds reference counting to C++ but we cannot use it until spring is modified to support this construct (the compiler generates duplicate `TypeInfo` and `TypeData`).
```cpp
    __fastcall ~TCppImpl1WithDelphiDep()
    {
      //We need to manually release the object
      m_dep->Release();
    }
  public:
    INTFOBJECT_IMPL_IUNKNOWN(TInterfacedObject);
  };
  #pragma pop explicit_rtti
```
Registration code, since we cannot use attributes, the registration must be done by code. We also cannot use generics since type explosion in Delphi code would be needed and it would only cause unknown symbol during linking. We may overcome this by adding another C++ wrapper written in native C++.
```cpp
  void Register(TContainer& container)
  {
    container.RegisterType(__delphirtti(TCppImpl1))
      .AsDefault()
      .AsSingleton();

    container.RegisterType(__delphirtti(TCppImpl1WithDelphiDep))
      .Implements(__delphirtti(IService1), "cppdep")
      .InjectField("m_dep");
  }
}
```
The application entry point, it just initializes the registration and redirects control to Delphi code.
```cpp
int _tmain(int argc, _TCHAR* argv[])
{
  CppUnit::Register(*InitApp());
  RunApp();
  return 0;
}
```

## Mobile (Android)

This was actually easier than I thought, I only had to create FMX Mobile project and add the *Android* target platform to the package and the compiler asked for the A file which got linked in the project SO. But unfortunetely this is where the "easy part" ends. The compiler fails to properly add some `TypeData` flags and the interfaces seems to be without a GUID which is not true so Spring will not work properly. *QC#126329*

No iOS tests for now, sorry. (Comments welcome)

## Problems
 * Build a lot, or some class constructors may not get called and the program would crash in runtime but build solves all,  that.
 * Static linking will generate `[ilink32 Error] Fatal: Illegal EXTDEF fixup index in module 'Spring.Container.Core.pas'` (the same error we get when we try to compile a single C++ project), this seems to be a compiler issue combined with how the units are created (exact problem cannot be simple pinpointed in code). *QC#126328*
 * ~~Debug DCUs with static linking (that we currently don't use) seems to only be compiler path problem.~~
 * Automatic reference counting for interfaces in C++. like I said above is not currently possible until Spring is modified to support multiple `TypeInfo`s for a single type.
 * Use VCL or FMX unless system libs won't work.
 * Some configurations may need to set *Generate all C++ builder files* (or similar option) in *Delphi compiler* settings.
 * I also had to amnually delete generated HPP files because while trying this on Adnroid, Delphi automatically generated HPPs for FMX which made the project unable to compile.

## Sources
For the sources just checkout the cppinterop branch (https://bitbucket.org/sglienke/spring4d/branch/feature/cppinterop). We worked together with Stefan and removed the need for CppWrapper.

## Changes
 * 2014-07-29 sources were modified after CppWrapper  dependency was removed