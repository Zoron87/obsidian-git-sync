Секция <objects> / <win-def:registry_object> - какой ключ/значение проверять
```xml
<objects>
    <win-def:registry_object id="oval:zxc:obj:1" version="1"
      comment="HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon : Shell">
      <win-def:hive>HKEY_LOCAL_MACHINE</win-def:hive>
      <win-def:key>SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon</win-def:key>
      <win-def:name>Shell</win-def:name>
    </win-def:registry_object>
</objects>
```

Секция 