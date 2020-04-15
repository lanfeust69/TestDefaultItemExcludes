# Paths starting with `./` in DefaultItemExcludes are not properly handled

This is probably a flaw in the implementation of [FileMatcher](https://github.com/microsoft/msbuild/blob/vs16.5/src/Shared/FileMatcher.cs).

Note that this behavior is more or less the same on Windows and Linux, as well as on Windows with the full-framework `msbuild.exe`.

```pwsh
$ git clean -xdf

$ dotnet build
Microsoft (R) Build Engine version 16.5.0+d4cbfca49 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.
...
  None Items : '.gitattributes;.gitignore;ReadMe.md'
...

$ dotnet build
...
  None Items : '.gitattributes;.gitignore;ReadMe.md'
...

$ git clean -xdf

$ dotnet build -p:WithDot=True
...
  None Items : '.gitattributes;.gitignore;Data\Foo.txt;ReadMe.md'
...

$ dotnet build -p:WithDot=True
...
None Items : '.gitattributes;.gitignore;bin\Debug\net48\TestDefaultItemExcludes.exe;bin\Debug\net48\TestDefaultItemExcludes.exe.config;bin\Debug\net48\TestDefaultItemExcludes.pdb;Data\Foo.txt;ReadMe.md'
...
```
