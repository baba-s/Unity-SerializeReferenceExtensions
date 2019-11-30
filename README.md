# Unity SerializeReferenceExtensions

https://qiita.com/tsukimi_neko/items/7922b2433ed4d8616cce  

上記サイト様で公開されている SubclassSelectorAttribute で  
型を指定しなくても使えるようにしたもの  

## 使用例

### Before

```cs
[SerializeReference, SubclassSelector(typeof(ICommand))]
ICommand command;

[SerializeReference, SubclassSelector(typeof(ICommand))]
ICommand[] commandArray = new ICommand[0];

[SerializeReference, SubclassSelector(typeof(ICommand))]
List<ICommand> commandList = new List<ICommand>();
```

### After

```cs
[SerializeReference, SubclassSelector]
ICommand command;

[SerializeReference, SubclassSelector]
ICommand[] commandArray = new ICommand[0];

[SerializeReference, SubclassSelector]
List<ICommand> commandList = new List<ICommand>();
```