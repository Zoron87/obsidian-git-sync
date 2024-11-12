```C#
ViewBag.Role = new SelectList(roleStorage.GetAll().AsEnumerable(), "Name", "Description");
```

где roleStorage.GetAll() - список, например, ролей

```C#
public class Role
    {
        public string Name { get; set; }
        public string Description { get; set; }
    }
```


В представлении выводим след. образом:
```C#
@Html.DropDownList("Role", null, "Выберите роль", htmlAttributes: new { @class = "form-control" })
```
