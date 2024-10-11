Еще в 2022 году .NET 7 получил поддержку для работы с tar файлами в базовой библиотеке классов. 

Эндрю Лок как обычно во всем разобрался и описал, как выполнять некоторые базовые операции с tar-файлами, как он обычно использую CLI-утилиту tar и как вместо этого использовать встроенную в .NET поддержку.

```C#
using System.Formats.Tar;
using System.IO.Compression;

string sourceDir = "./my-files";
string outputFile = "./myarchive.tar.gz";

using FileStream fs = new(outputFile, FileMode.CreateNew, FileAccess.Write);
using GZipStream gz = new(fs, CompressionMode.Compress, leaveOpen: true);

TarFile.CreateFromDirectory(sourceDir, gz, includeBaseDirectory: false);
```

https://andrewlock.net/working-with-tar-files-in-dotnet/