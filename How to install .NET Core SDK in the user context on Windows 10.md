# How to install the .NET Core SDK in the user context on Windows 10

## Introduction

Many of us have machines that are given to us by our employers and are locked down to prevent us downloading useful applications that can make our lives easier.  Many of these machines are restricted from executing applications where administrative privileges are required.  This guide shows you how you can get up and running with the .NET Core SDK on such a machine and create a simple console application.

## Download .NET Core SDK

.NET Core has a useful web installer that will automatically install and create the correct environment for you.  In the case where you cannot download executables due to security policy, you will have to do this manually.

Download the zip package from here:

- https://dotnet.microsoft.com/download/dotnet-core

Ensure that you select the SDK binaries package rather than the installer for Windows (x64 or x86 for 64-bit or 32-bit).  Extract the contents of the zip file into a folder on your machine (e.g. `%USERPROFILE%\\.dotnet`).

### Set Environment Variables

In order for the CLI to work and for apps to use .NET Core you will need to set some environment variables.  Fortunately you can set these specifically for your user and not require administrative rights.  

On Windows 10 click the start button and type **env**.  You should be presented with two options, select the option that says **Edit environment variables for your account**.  Use the **New** button to create a new variable called `DOTNET_ROOT` with the value `%USERPROFILE%\\.dotnet` (or whatever folder you extracted the zip file into).

Next edit the **Path** environment variable and add a new entry `%USERPROFILE%\\.dotnet` (again substitute with the correct path if you extracted to a different folder).  Move this entry to the top of this list using the **Move Up** button so that your newly installed version of the SDK is located first.

If there is already a version of the .NET Core Runtime installed on your system, and you prefer your newly installed SDK to be selected when running .NET Core apps, then you will need to add another environment variable, `DOTNET_MULTILEVEL_LOOKUP` with the value `0`.  This is because .NET Core will prefer to use any globally installed versions of the .NET Core Runtime over locally installed versions, and this setting overrides that behaviour.

## Create a simple console App

Open a command prompt and execute the following commands:

```shell
mkdir C:\Temp\HelloWorld

cd C:\Temp\HelloWorld

dotnet new console
```

If you have installed .NET Core correctly then you should get the following output:

```shell
Running 'dotnet restore' on C:\Temp\HelloWorld\HelloWorld.csproj...

  Determining projects to restore...

  Restored C:\Temp\HelloWorld\HelloWorld.csproj (in 145ms).



Restore succeeded.
```

Now lets try your new application.  To run it execute the following command:

```shell
dotnet run
```

If all is well, after a small delay you should see the following output:

```shell
Hello World!
```

### Extending the app

To update your new console application you can open the file `Program.cs` inside your folder with any plain text editor of your choosing.  

When you open the file you will see the following:

```csharp
using System;

namespace HelloWorld
{
  class Program
  {
    static void Main(string[] args)
    {
      Console.WriteLine("Hello World!");
    }
  }
}	
```

Change the word **World** inside the string to **Universe** and save the file.  Then to execute run the same command as before:

```shell
dotnet run
```

and observe the new output:

```shell
Hello Universe!
```

## What's Next

Now you can build a simple console application without requiring administrative privileges on your Windows 10 machine.  You can even try some of the other project templates that are available from the `dotnet new` command such as:

- Blazor Web Assembly (*blazorwasm*)
- ASP.NET Core with Angular (*angular*)
- ASP.NET Core with React.js and Redux (*reactredux*)

Local restrictions on your desktop may prevent you from running these types so it may be a matter of trial and error.

To get a list of installed templates on your machine execute the `dotnet new --list` command.