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

## 補足

下記のようなコードを使用することで  
SubclassSelectorAttribute が付与されている変数の型を自動で抽出しています  

```cs
public static Type GetType( SerializedProperty property )
{
    const BindingFlags bindingAttr =
            BindingFlags.NonPublic |
            BindingFlags.Public |
            BindingFlags.FlattenHierarchy |
            BindingFlags.Instance
        ;

    var propertyPaths = property.propertyPath.Split( '.' );
    var parentType    = property.serializedObject.targetObject.GetType();
    var fieldInfo     = parentType.GetField( propertyPaths[ 0 ], bindingAttr );
    var fieldType     = fieldInfo.FieldType;

    // 配列もしくはリストの場合
    if ( propertyPaths.Contains( "Array" ) )
    {
        // 配列の場合
        if ( fieldType.IsArray )
        {
            // GetElementType で要素の型を取得する
            var elementType = fieldType.GetElementType();
            return elementType;
        }
        // リストの場合
        else
        {
            // GetGenericArguments で要素の型を取得する
            var genericArguments = fieldType.GetGenericArguments();
            var elementType      = genericArguments[ 0 ];
            return elementType;
        }
    }

    return fieldType;
}
```

- 変数、配列、リストで正常に動作することを確認しています
- 名前空間が適用されているクラスで正常に動作することを確認しています