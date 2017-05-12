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

### 変数

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
  // O
  let optionalValue: String?


```

## 定数
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
- Playgroundの場合はMarkdown適用が可能なので積極的に利用する
- コメントは [Apple](https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/AddingMarkup.html#//apple_ref/doc/uid/TP40016497-CH100-SW1)の標準規約を守る
  - Quick Helpにも反映されるしドキュメント化も可能

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

> Apple Format  

```
/**
  Another description

     - important: Make sure you read this
     - returns: a Llama spotter rating between 0 and 1 as a Float
     - parameter totalLlamas: The number of Llamas spotted on the trip

 More description
 */
```

## Protocol, Delegate
- ProtocolとDelegateの用途を明確にして名名する


## その他

- ParentControllerからChildViewControllerの@IBoutletを参照する時要注意！
- ChildViewControllerのIBOutletも必ずOptional bindingで参照する
  - クラッシュの原因
- Protocolでお互いに通信する

- Objectはinitで生成

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











———————————
내가 생각하는 것

코딩규약의 의도
- 통일성 있는 소스
- 최소한의 코딩으로 다른 엔지니어에게 모든 정보를 전달할 수 있는 가장 효율적이라고 생각하는 방법을 정의

가급적이면 NSObject씨리즈를 쓰지 않는다.

변수정의
변수/정수

문자열

func정의
- 기본적으로 파라메터는 명사를 씀.
- 전치사를 이용해서 구체적으로 정의를 하고 싶은 경우에는 전치사를 아규먼트로 넣음.
    - 아규먼트의 활용예?? 패턴을 나열해 볼 것

Optional사용
- 캐스팅, Dic의 값 참조등 nil이 들어올 가능성이 조금이라도 있을 경우 Optional을 사용
- 단, 강제참조는 @IBOutlet이외엔 지양한다.
    - ContainerView처럼 부모Controller에서 자식Controller의 IBOutlet을 참조하는 처리의 경우 Optional binding을 이용해서 참조하도록 함.

protocol, delegate
- 사양이 명확한 경우에는 프로토콜/ 프로토콜 상속/ 프로토콜의 확장을 이용
- 사용목적에 따라서 말머리에 명확하게 씀.

Extension활용
- 간단히 Cocoa framework쪽 Class, Struct의 확장뿐만 아니라.
- Delegate도 이것을 이용해서 따로 정의

Generic활용
- 특정형에 구애받지 않고 정의하는 경우

Private, fileprivate등 접근제한자 사용법
- func, property등 목적에 맞게 제한자를 걸어서 정의
    - 심지어 @IBOutlet도!

로컬라이징?
 - 문자열은 didSet에 이용

리소스관리?
- R.swift를 이용
- ViewController, Segue, Images, String, Stroyboard, Nib등 리소스 관리를 위한 정의를 일일이 하지 않아도 됨.
    - 관련 문서 https://github.com/mac-cain13/R.swift


디자이너블은 지양
- Extension으로 만들어 두는건 지향?
    - 퍼포먼스 저하의 원인?
- Xcode의 퍼포먼스 저하의 원인..

코멘트
https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/AddingMarkup.html#//apple_ref/doc/uid/TP40016497-CH100-SW1
- 코멘트에 관해서는 애플 문서를 준수하자.
- 간단히 /// 를 이용해서 써놓으면 Quick Help에 만영이 됨.
- 옵션클릭으로 간단히 설명을 볼 수 있어야함.
- 설명 파라미터랑 리턴값 정도만 적으면 될 듯..
    - 그외 설명을 부가하고 싶을 경우엔??
- http://nshipster.com/swift-documentation/
    - 이사람꺼도 괜찮음.
-
