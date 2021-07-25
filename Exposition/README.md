# 만국박람회

### 프로젝트 기간 : 21.07.05 ~ 21.07.16

### 프로젝트 인원 : 3명

### 프로젝트 진행 방식 : 페어 프로그래밍

### - Navigator, Driver, Observer

- 관찰자가 커밋이 끝났을 때 커밋내용 정리하고 납득시키기

- 관찰자가 질문할 것들 기록하기 (다른 사람들도 기록 가능)

- Navigator → Driver

- Driver → Observer

- Observer → Navigator

---

## 모델타입 UML

### 수정 전

<img width="526" alt="스크린샷 2021-07-21 오후 4 53 53" src="https://user-images.githubusercontent.com/70311145/126452446-bc918016-eaae-43f1-aa89-ebf277d8de42.png">

<img width="961" alt="스크린샷 2021-07-21 오후 4 59 17" src="https://user-images.githubusercontent.com/70311145/126453142-1c2e52ad-09f7-40c8-8d23-60b376a6402e.png">

- 고민했던 부분

  - 프로토콜은 어떻게 연결할까??

  - 중첩타입은 어떻게 연결할까??

- 해결방안

  여러 참고 자료들을 알아보았다.

  고민을 했던 이유는 아래 두 자료를 공부하면서 마름모 모양으로 중첩을(집약 관계) 나타내고 있었기때문이다.

  [독서 중 본 내용](https://github.com/jaemuYeo/iOS_Study/blob/main/TIL/4_week/2021-06-20.md) <- 여기서 부터 의문점이 생기기 시작했다.

  [TIL에 써놓은 자료](https://github.com/jaemuYeo/iOS_Study/blob/main/TIL/5_week/2021-06-22.md)

  [stackoverflow](https://stackoverflow.com/questions/27146519/private-nested-java-class-in-uml-diagram)

  [자료1](https://newbedev.com/how-to-represent-the-nested-class-of-c-in-uml)

  [자료2](https://sparxsystems.com/resources/tutorials/uml2/class-diagram.html)

  캠프에 질문을 통해 수박의 답변으로 얻은 답은 마름모의 경우 계층 관계가 분명할 때,

  동그라미더하기의 경우 계층관계를 가지지 않지만 상위 타입이 하위 타입을 포함할 때 사용한다.

  프로토콜은 Raywnderlich에서 너무나 설명을 잘 해놓아서 쉽게 적용할 수 있었다.

  [raywenderlich 참고자료](https://www.raywenderlich.com/books/design-patterns-by-tutorials/v3.0/chapters/2-how-to-read-a-class-diagram)

  그 외에 일반 실선으로 연결되어있던 의존관계 부분도 점선으로 변경하였다.

### 수정 후

<img width="947" alt="스크린샷 2021-07-22 오전 3 18 00" src="https://user-images.githubusercontent.com/70311145/126539387-f1dff139-2948-47b8-9d02-dee07107d64e.png">

---

## Step1

### JSON(JavaScript Object Notation)

`"Key" : Value`로 이루어진 데이터들의 집합

```JSON
{
    "name":"잼킹",
    "image_name":"jamking",
    "short_desc":"잼킹은 2기 캠퍼입니다",
    "desc":"블라블라,,,"
}
```

- JSON 데이터는 이름과 값의 쌍으로 이루어진다.

- JSON 데이터는 쉼표(,)로 나열된다.

- 객체(object)는 중괄호({})로 둘러쌓아 표현한다.

- 배열(array)은 대괄호([])로 둘러쌓아 표현한다.

**JSON의 기본 자료형**

숫자(number), 문자열(string), 불리언(boolean), 객체(object), 배열(array), NULL

### 디코딩과 인코딩

인코딩/디코딩은 정보의 형태나 형식을 변환하는 처리에 대해 표준화하고

보안, 처리 속도 향상, 저장 공간 절약 등의 목적으로 사용한다.

많은 프로그래밍 작업에는 네트워크 연결을 통한 데이터 전송, 디스크에 데이터 저장 또는 API 및 서비스에

데이터 제출이 포함되며 이러한 작업을 수행하려면 데이터가 전송되는 동안 중간 형식간에 데이터를 인코딩하고 디코딩해야한다.

- `JSONDecoder` - JSON 데이터에서 Swift 타입의 인스턴스로 디코딩

- `JSONEncoder` - Swift 타입의 인스턴스를 JSON 데이터로 인코딩

### Codable

Codable은 디코딩과 인코딩을 모두 사용할 수 있다.

혹여나 Decodable만 필요로 할 경우 Codable을 채택하면 조금 무거워 질 수 있는 단점이 있다.

```swift
typealias Codable = Decodable & Encodable
```

### CodingKey

JSON 형태의 데이터로 상호 변환하고자 할 때는 인코딩/디코딩할 JSON 타입의 Key와 사용자 정의 프로퍼티가 일치해야한다.

JSON의 키 이름을 프로퍼티의 이름과 다르게 표현하려면 타입 내부에 String 타입의 원시값을 갖는 CodingKeys라는

이름의 열거형을 선언 후 CodingKey 프로토콜을 준수해야한다.

```swift
struct Item: Decodable {
    let name: String
    let imageName: String
    let shortDescription: String
    let description: String

    private enum CodingKeys: String, CodingKey {
        case name
        case imageName = "image_name"
        case shortDescription = "short_desc"
        case description = "desc"
    }
}
```

---

## Step2

![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/70311145/126492052-c5abeaac-f5d2-4ac6-8e96-11d2951239e1.gif)

---

### TableView

테이블뷰는 행과 섹션으로 분할된 세로 스크롤 콘텐츠의 단일 열을 표시한다.

```swift
@MainActor class UITableView : UIScrollView
```

- UITableViewDataSource

  개체가 데이터를 관리하고 테이블뷰에 대한 셀을 제공하기 위해 채택한다.

  TableView는 DataSource를 채택하지 않으면 동작하지 않는다.

  ```swift
  // 테이블뷰의 지정된 섹션에 있는 행 수를 반환하도록 데이터 소스에 지시하는 메서드
  func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int

  // 테이블뷰의 특정 위치에 삽입할 셀에 대한 데이터 소스를 요청하는 메서드
  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell
  ```

- UITableViewDelegate

  선택 항목을 관리하고, 섹션 머리글과 바닥글을 구성하고, 셀을 삭제 및 재정렬하고, 테이블뷰에서 기타 작업을 수행한다.

  ```swift
  // delegate에게 행이 선택 되었음을 알리는 메서드
  optional func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath)
  ```

---

### 커스텀 셀

기본으로 제공하는 셀의 스타일이 없을 경우 셀을 커스텀하여 구현한다.

[예전에 정리했던 블로그 참고](https://jaemuyeo.github.io/ios/tableView/)

---

### ScrollView AutoLayout

1. 루트뷰에 스크롤뷰를 올리고 슈퍼뷰에 맞춰 왼쪽,오른쪽,위,아래를 전부 0으 로 해준다.

2. 스크롤뷰 안에 View를 하나 넣어서 이름을 ContentsView로 바꿔준다.(Optional)

3. 컨텐츠뷰를 스크롤뷰와 크기를 같게 맞춰준다. (아직까지는 제약에러가 뜬다.)

   - 컨텐츠뷰 안에 컨텐츠가 없기때문

4. 스크롤을 가로로 하려면 컨텐츠뷰를 스크롤뷰와 높이를 맞춰주고, 세로로 스크롤하려면 너비를 맞춰준다.

5. 컨텐츠뷰안에 원하는 컨텐츠(View)를 넣어서 컨텐츠뷰에게 제약을 걸어준다.

---

### DataAsset Parsing POP적용

[Contents.json File](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/Contents.html#//apple_ref/doc/uid/TP40015170-CH36-SW1)

단계별로 변화

```swift
// 처음 시도 - VC에서 직접 Parsing
protocol JSONDecodable {
    func decodeData(assetName: String)
}

class ViewController: UIViewController {
    var exposition: Exposition?
}

extension ViewController: JSONDecodable {
    func decodeData(assetName: String) {
        let decoder = JSONDecoder()
        guard let assetData = NSDataAsset(name: assetName) else {
            return
        }
        do {
            exposition = try decoder.decode(Exposition.self, from assetData.data)
        } catch {
            // 에러처리
        }
    }
}
```

```swift
// 두 번째 시도 - VC에서 디코딩하지 않고 JSONDecadable이 Parsing
protocol JSONDecodable {
    associatedtype T: Decodable
}

extension JSONDecodable {
    func decodeJSON(fileName: String) throws -> T {
        guard let convertedAsset = NSDataAsset(name: fileName) else {
            throw DecoderError.invaildFileName
        }
        do {
            let decoder = JSONDecoder()
            return try decoder.decode(T.self, from: convertedAsset.data)
        } catch {
            throw error
        }
    }
}

class ViewController: UIViewController {
    let expo = "Asset File Name"
}

extension ViewController: JSONDecodable {
    typealias T = Exposition

    func decode() {
        titleLabel.text = try? decodeJSON(fileName: expo).title
    }
}
```

```swift
// 세 번째 시도 - associatedtype 제거하고, 함수에 제네릭 적용
protocol JSONDecodable {

}

extension JSONDecodable {
    func decodeJSON<T: Decodable>(fileName: String) throws -> T {
        guard let convertedAsset = NSDataAsset(name: fileName) else {
            throw DecoderError.invaildFileName
        }

        do {
            let decoder = JSONDecoder()

            return try decoder.decode(T.self, from: convertedAsset.data)
        } catch {
            throw error
        }
    }
}
```

```swift
// 이렇게도 가능
func decodeJSON<T>(fileName: String) throws -> T where T : Decodable {}
```

---

### 뷰 간 데이터 전달

처음에는 prepare 메서드를 사용하여 세그를 활용해 데이터를 전달하였다.

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let indexPath = itemListTableView.indexPathForSelectedRow,
            let itemDetailView = segue.destination as? ItemDetailViewController else {
        return
    }
    itemDetailView.setItem(item: items[indexPath.row])
}
```

하지만 VC간 의존도가 끈끈하다 생각이 들어 Delegate를 통해 적용할 수 있지 않을까? 생각해보았다.

```swift
protocol SendDataDelegate: AnyObject {
    func sendData(data: Item)
}

class ItemListViewController: UIViewController {
    weak var sendDataDelegate: SendDataDelegate?
}

extension ItemListViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        let dataItem = items[indexPath.row]
        guard let detailVC: ItemDetailViewController = storyboard?.instantiateViewController(identifier: "secondVC") else {
            return
        }
        sendDataDelegate = detailVC
        navigationController?.pushViewController(detailVC, animated: true)
        sendDataDelegate?.sendData(data: dataItem)
    }
}

class ItemDetailViewController: UIViewController {
    private var item: Item?

    func setItem(item: Item) {
        self.item = item
    }
}

extension ItemDetailViewController: SendDataDelegate {
    func sendData(data: Item) {
        setItem(item: data)
    }
}
```

Delegate를 통해 데이터 전달하는 방법을 구현했지만 리뷰 받은 내용에서 두 VC의 관계가

코드에만 없을 뿐 스토리보드에 존재한다는 것을 알게되었다.

ViewController간의 이동은 segue, 스토리보드, 코드로 직접 이동 등 피할 수 없는 관계를 가지므로

어떠한 방법을 써도 관계를 끊지는 못한다,,

그렇기에 prepare 메서드를 사용해도 좋은 방법이라 그대로 사용하기로 했다.

---

### 상수 관리

매직넘버와 모호한 데이터 타입의 값들을 바로 넣기보다 데이터 타입 확장 또는 구조체, 열거형의 `static let`으로

관리하는 것을 습관화 하기로 했다.

처음에는 String들의 모음들을 전부 extension을 통해 static let으로 관리했었는데

그 방법보다는 객체 내에 private으로 접근 제어를 두고 상수 속성에 따라 다른 struct를 구성하여 관리하는 방법을 추천 받았다.

```swift
class ViewController: UIViewController {
  private struct Constant {
    static let fileName = "Item"
    static let visitor = "방문객"
  }

  private struct Metric {
    static let one = 1
  }
}
```

`static 변수들은 lazy 속성이 있기 때문에 최초 접근 전에는 초기화 되지 않는 장점이 있다.`

다만 말씀해주신 구조체로 관리하기 보다 열거형으로 static let을 만들어 사용하면

실수로 인스턴스화할 수 없고 순수한 네임스페이스로 작동한다.

또한 굳이 범용적으로 사용하지 않을거라면 단일로 사용하는 객체 또는 메서드의 스코프 내부에 프로퍼티로 관리해도 좋다.

`ex) cell의 identifier를 상수로 관리`

---

### Parsing Unit Test

Given, when, then 패턴으로 테스트 시도.

다만 처음에는 인덱스의 첫 번째 값이 일치한지, 마지막 값이 일치한지에 대한 테스트만 시도했었다.

```swift
func test_파일이름이items일때_Item의첫번째요소의타이틀은_직지심체요절이다() {
    //Given
    let fileName = "items"
    let expectedInputValue = "직지심체요절"
    //When
    let expectedResult = try? cutWithItems.decodeJSON(fileName: fileName).first?.name
    //Then
    XCTAssertEqual(expectedInputValue, expectedResult)
}
```

이럴 경우 현재는 정적인 데이터 요소를 테스트 중이지만 만약 `동적`인 데이터라면 실패할 가능성이 커진다.

그래서 필수로 있어야하는 요소라면 contains메서드를 활용하면 좋다.

```swift
func test_파일이름이items일때_name이_직지심체요절인요소가있다() {
    //Given
    let fileName = "items"
    let expectedInputValue = "직지심체요절"
    //When
    let decodedItems = try? cutWithItems.decodeJSON(fileName: fileName)
    let expectedResult = decodedItems?.contains { item in
        item.name == expectedInputValue
    }
    //Then
    XCTAssertTrue(expectedResult == true)
}
```

---

### Navigation Stack에서 특정 VC만 NavigationBar 숨김처리

```swift
override func viewWillAppear(_ animated: Bool) {
super.viewWillAppear(animated)
self.navigationController?.isNavigationBarHidden = true
// 또는 self.navigationController?.navigationBar.isHidden = true
}
```

위 코드는 해당 navigationController로 묶인 모든 ViewController들의

navigationBar를 숨기므로 disAppear에서 다시 false로 해줘야한다.

```swift
override func viewWillDisappear(_ animated: Bool) {
    super.viewWillDisappear(animated)
    self.navigationController?.isNavigationBarHidden = false
    // 또는 self.navigationController?.navigationBar.isHidden = false
}
```

---

## Step3

### 특정 화면 회전 고정하기

### 첫 번째 방법

1. AppDelegate에서 아래와 같이 코드 작성

   ```swift
   class AppDelegate: UIResponder, UIApplicationDelegate {
   //화면회전을 잠그고 고정할 목적의 플래그 변수를 추가
   var shouldSupportAllOrientation = true

   ,,,

   func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
       if (shouldSupportAllOrientation == true){
           return UIInterfaceOrientationMask.all
           // 모든방향 회전 가능
       }
       return UIInterfaceOrientationMask.portrait
           // 세로방향으로 고정.
       }
   }
   ```

2. 세로로 고정하고자 하는 VC에 코드 작성

   viewDisapper에서 true로 해주지 않을 경우 모든 VC에 적용된다.

   ```swift
   class MainViewController: UIViewController {
       let appDelegate = UIApplication.shared.delegate as? AppDelegate

       override func viewWillAppear(_ animated: Bool) {
           appDelegate.shouldSupportAllOrientation = false
       }

       override func viewWillDisappear(_ animated: Bool) {
           appDelegate.shouldSupportAllOrientation = true
       }
   }
   ```

### 두 번째 방법

AppDelegate 대신 override를 이용해서 처리하는 코드.

이 방법에서 얘기하는 특정 화면의 단위는 NavigationController로 연결된 모든 VC이다.

```swift
extension UINavigationController {
    open override var shouldAutorotate: Bool {
        if self.topViewController is MainViewController {
            return false
        }
        return true
    }

    open override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
        return .portrait
    }
}
```

### `Issues`

NavigationController 스택에 쌓이지 않아도 NavigationController의 extension에 영향을 받는 VC

- Navigation Stack들을 세로 고정으로 모두 제한하고, 마지막 VC에 새로운 VC를

  present를 통해 이동하여 모달로 띄웠는데 stack에 쌓이지 않을 거라 예상했지만 똑같이 세로 고정이 적용됨.

왜 영향을 받을까??

ios13부터는 .pageSheet으로 변경이 되었는데 이 영향일 가능성이 있다.

[공식문서 pageSheet](https://developer.apple.com/documentation/uikit/uimodalpresentationstyle/pagesheet)

[zedd님 블로그](https://zeddios.tistory.com/828)

.pageSheet형태로 present 되었을 때, 뷰 디버깅을 해보면 뷰계층에 UINavigationController가 포함되어 있다.

present를 통해 이동하는 VC의 화면을 .fullScreen으로 띄우면 뷰계층에 해당 UIViewController만 존재하고 위의 이슈에 영향이 없음!!

---

### AccessibilityLabel과 Value

- 단순 정보를 전달할 때는 accessibilityLabel을 통해 값을 전달

- UISwitch / UITextField 등 사용자의 입력에 따라 값이 변하는 경우는 accessibilityValue를 통해 값을 전달

---

## 해결하지 못한 부분

Accessibilitylabel Font Size를 최대로 올렸을 때 Label과 Button이 겹치는 문제

<img width="333" alt="스크린샷 2021-07-22 오전 4 08 25" src="https://user-images.githubusercontent.com/70311145/126545792-3ad19912-534d-4426-a2f1-690bb47fb0d5.png">
