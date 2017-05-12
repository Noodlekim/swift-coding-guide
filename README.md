# Swift3 コディング規約

## 規約目的
- 統一性があるソース
- 世の中で一番一般的に適用されているルールで他のエンジニアの可読性も考慮
- 最低限のコーディングで最大の情報を伝えられるソースを目指す

## Reference
- [The Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
- [Apple](https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html)
- [raywenderlich](https://github.com/raywenderlich/swift-style-guide/blob/master/README.markdown)

## 変数定義

- 形は省略する
  - Optionalの場合は指定する

```
  // O
  let value = "String value"

  // X
  let value: String = "String value"

  // O
  let flag = true

  // X
  let flag: Bool = false

  // O
  let optionalValue: String?

```

## 定数定義
- 定数名で理解できる名名にする
- 定数のタイプは指定しない

```
let notChangeString = "Hoge"
let notChangeArray = ["Hoge", "Fuga"]
let notChangeDictinary = ["Hoge": "Fuga"]
```


## Optional
- 初期値は省略する
- 基本的にOptional bindingを利用してnilチェックをする
- 強制参照をしない

```
// X
var optionalValue: String? = nil

// O
var optionalValue: String?

// X
let url = URL.init(string: "https://github.com")
if url != nil {
  let request = NSURLRequest.init(url: url!)
}

// X
let url = URL.init(string: "https://github.com")
let request = NSURLRequest.init(url: url!)

// O
var value: String? = "string value"
if let value = value {
    print(value)
}

// O
if let url = URL.init(string: "https://github.com") {
  let request = NSURLRequest.init(url: url)  
}
```

## ClassかStructureか
- 継承を利用する必要がある場合はClassで定義
- それ以外はStructureとProtocol, Extensionを利用する

## Enumerations
- 項目は小文字で
- なるべくSwitch文で判断させる

```
enum Type: String {
    case japan = "ja"
    case korea = "kr"
}

if let type = Type.init(rawValue: "ja") {
    switch type {
    case .japan:
        print("japan")
    case .korea:
        print("korea")
    default: break
    }
}
```

## コメント
- ClassやStructureには概要を必ず記述する

```
// X
// This is ClassA.
class ClassA {
    ...
}

// O
/// This is ClassA.
class ClassA {
    ...
}
```
- コメントは [Apple](https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/AddingMarkup.html#//apple_ref/doc/uid/TP40016497-CH100-SW1)の標準規約を守る
- Quick Helpにも反映されるしドキュメント化も可能
  
```
/**
  Another description

     - important: Make sure you read this
     - returns: a Llama spotter rating between 0 and 1 as a Float
     - parameter totalLlamas: The number of Llamas spotted on the trip

 More description
 */
```

- Playgroundの場合はMarkUp適用が可能なので積極的に利用する

```
//: Title

/*:
 ### This is Playground

 */
```

## Protocol, Delegate
- ProtocolとDelegateの用途を明確にして名名する

## キャスティング
- ダウンキャスティングはOptionalを利用する

```
class Parent {
  func test(){ }
}

class Child: Parent {
    
}

let child = Child.init()

// O
if let child = child as? Parent {
    print("ここで書く")
}

// X
if child is Parent {
    (child as Parent).test()
}

// X
if child.isKind(of: Parent) {
    (child as Parent).test()
}
```


## その他

### ParentControllerからChildViewControllerの@IBoutletを参照する時要注意！
- ChildViewControllerのIBOutletも必ずOptional bindingで参照する
  - クラッシュの原因
- Protocolでお互いに通信する

### Objectはinitで生成

```
class ClassA {
    let value: String
    required init(value: String) {
        self.value = value
    }
}

// X
let classB = ClassA(value: "value")

// O
let classA = ClassA.init(value: "value")
```

### リソース管理はR.swfitでする
[R.swift](https://github.com/mac-cain13/R.swift)を利用してリソース管理にかかる工数を下げる
- Controller, Segue, Images, String, Storyboard, Nibなどのリソース管理が自動化される

<br>
<br>
<br>  

## 改版履歴
- 2017.5.12　基本コーディングルール記述
