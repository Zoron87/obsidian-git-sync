Для работы протокола Quic на win10 необходимо получить его из OpenSSL, для этого:
1. Нужно предварительно установить CMake
2. Strawberry Perl
3. Visual Studio 2019/2022 C++ tools
4. Установить Powershell 7
   ```powershell
  winget install --id Microsoft.PowerShell --source winget
   
  #Убедиться в установке
  pwsh --version
   ```
5. Выполнить скрипт в powershell
```powershell
git clone --recurse-submodules https://github.com/microsoft/msquic.git  
cd msquic  
  
pwsh ./scripts/prepare-machine.ps1  
pwsh ./scripts/build.ps1 -Config Debug -Arch x64 -Tls openssl

# или Release версию
pwsh ./scripts/build.ps1 -Config Release -Arch x64 -Tls openssl
```

## Если зависает на этапе "Download JOM"
то скачать пакет самостоятельно
```powershell
- `https://qt.mirror.constant.com/official_releases/jom/jom_1_1_3.zip`
    
- `https://mirrors.ocf.berkeley.edu/qt/official_releases/jom/jom_1_1_3.zip`
```
## Где лежит готовый `msquic.dll`

У MsQuic build output идёт в `artifacts`, и в документации прямо указан шаблон:

./artifacts/bin/{platform}/{arch}_{config}_{tls}

Вполне возможно, что понадобится забрать не только dll файл, а и его OpenSSL-зависимости.

Либо можно попробовать забрать dll библиотеку из готовой сборки
https://www.nuget.org/packages/Microsoft.Native.Quic.MsQuic.OpenSSL