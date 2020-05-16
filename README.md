# In Linux

> We start with files:
> ```
> util.go
> ```

This will produce `util.dll` and `util.h` :
```bash
GOOS=windows \
CC=x86_64-w64-mingw32-gcc \
CGO_ENABLED=1 \
go build -v -o util.dll -buildmode=c-shared util.go
```

# In Windows

> We start with files:
> ```
> util.dll
> util.h
> main.c
> ```

Invoking `gcc` in CMD will produce a `mmautil.dll`:
```bash
gcc -pthread -shared -o mmautil.dll
 -I"{$BASE_PATH}\IncludeFiles\C"
 -I"{$BASE_PATH}\Links\MathLink\DeveloperKit\Windows-x86-64\CompilerAdditions"
 main.c
 -L"{$BASE_PATH}\Links\MathLink\DeveloperKit\Windows-x86-64\CompilerAdditions"
 -L"{$BASE_PATH}\Libraries\Windows-x86-64"
 -L. -lutil
```

Or in Mathematica, the following command will produce a `liblink_util.dll`:
```wl
CreateLibrary[
 File@"main.c"
 , "liblink_util"
 , "CompileOptions" -> "-pthread"
 , "TargetDirectory" -> Directory[]
 , "LibraryDirectories" -> {Directory[]}
 , "Libraries" -> {"util"}
 ]
```
(`mmautil.dll` and `liblink_util.dll` are basically byte-identical.)

Now we can load functions from `mmautil.dll` in Mathematica through `LibraryLink` .
