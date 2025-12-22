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

Секция <tests> / <registry_test> - как проверять (существует / соответствует шаблону)
```xml
<tests>
    <win-def:registry_test id="oval:zxc:tst:1" version="1"
      check="at least one" check_existence="at_least_one_exists"
      comment="Winlogon Shell contains explorer.exe">
      <win-def:object object_ref="oval:zxc:obj:1"/>
      <win-def:state state_ref="oval:zxc:ste:1"/>
    </win-def:registry_test>
</tests>
    ```
Секция <states> / 