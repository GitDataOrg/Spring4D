**Required Version**: 1.2


This is a small example on how to achieve catching unhandled exceptions via explicit decorator or interception.

The benefit of an explicit decorator is that this can easily done via [Pure DI](http://blog.ploeh.dk/2014/06/10/pure-di/) but it requires writing (or generating) extra boilerplate code and registration for each type.

The benefit of the interception approach is that you only write an interceptor once and can apply it to any registered type. However this might involve some runtime performance impact on methods that are getting called very often and also depends on the parameterss being passed (more parameters means slower performance because they are getting marshalled to a TValue).


```
#!delphi

program ExceptionDecorator;

{$APPTYPE CONSOLE}

uses
  Spring.Container,
  Spring.Interception,
  SysUtils;

type
  IService = interface(IInvokable)
    ['{CC545714-73B7-4CDA-AFEA-44A46FF2C744}']
    procedure Run;
  end;

  TService = class(TInterfacedObject, IService)
    procedure Run;
  end;

  TServiceDecorator = class(TInterfacedObject, IService)
  private
    fDecoratee: IService;
  public
    constructor Create(const decoratee: IService);
    procedure Run;
  end;

  TExceptionInterceptor = class(TInterfacedObject, IInterceptor)
  public
    procedure Intercept(const invocation: IInvocation);
  end;

procedure Main;
const
  Option = 1;
begin
  case Option of
    1: // explicit decorator
    begin
      GlobalContainer.RegisterType<IService,TService>;
      GlobalContainer.RegisterDecorator<IService,TServiceDecorator>;
    end;
    2: // via interceptor
    begin
      GlobalContainer.RegisterType<IService,TService>.InterceptedBy<TExceptionInterceptor>;
      GlobalContainer.RegisterType<TExceptionInterceptor, TExceptionInterceptor>;
    end;
  else
    GlobalContainer.RegisterType<IService,TService>;
  end;

  // build and run
  GlobalContainer.Build;
  GlobalContainer.Resolve<IService>.Run;

  // if the exception was properly handled the program will reach the following line
  Writeln('finished');
end;

procedure TService.Run;
begin
  raise EProgrammerNotFound.Create('Whoops!');
end;

{ TServiceDecorator }

constructor TServiceDecorator.Create(const decoratee: IService);
begin
  inherited Create;
  fDecoratee := decoratee;
end;

procedure TServiceDecorator.Run;
begin
  try
    fDecoratee.Run;
  except
    on E: Exception do
      Writeln('An unhandled exception occured: ', E.Message);
  end;
end;

{ TExceptionInterceptor }

procedure TExceptionInterceptor.Intercept(const invocation: IInvocation);
begin
  try
    invocation.Proceed;
  except
    on E: Exception do
      Writeln('An unhandled exception occured: ', E.Message);
  end;
end;

begin
  try
    Main;
  except
    on E: Exception do
      Writeln(E.ClassName, ': ', E.Message);
  end;
  Readln;
end.
```