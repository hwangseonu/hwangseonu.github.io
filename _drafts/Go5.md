### 사용자 지정 타입

위의 기본 타입 외에 사용자 지정 타입을 사용할 수 있다.  
Go언어에서는 `type` 이라는 키워드를 통해 기본 타입에 별칭을 붙여 사용하거나  
`인터페이스 (interface)`, `구조체 (struct)` 등에 이름을 붙여 사용할 수 있다.  

#### type 키워드

Go는 타입에 별칭을 붙여 용도를 나타낼 수 있다.  
대표적으로 기본 타입의 `byte`는 `uint8`의 별칭이다.  

```go
type byte = uint8
type rune = int32

type Age = uint16
```

#### struct (구조체)

`struct(구조체)`는 여러 필드들의 집합이다.  
Go언어를 사용하면서 커스텀 타입으로 가장 많이 사용되는 타입이다.  

`struct` 내부에는 여러개(0개 포함)의 필드를 정의할 수 있으며  
`대문자`로 시작하는 경우 외부(다른 패키지)의 접근이 가능하고  
`소문자`로 시작하는 경우 외부(다른 패키지)의 접근이 `차단`된다

```go
type Person struct {
    Name string // Public
    Age uint16 // Public
    secret string // Private: 다른 패키지에서 접근할 수 없음
}

var p = Person{
    "hwangseonu", 22, "***"
} 

fmt.Println(p.Name)
fmt.Println(p.secret) // 같은 패키지라면 접근 가능
```

`struct`는 다른 `struct`를 `임베딩` 할 수 있는데  
마치 Java의 `상속` 처럼 `다른 struct`의 필드를 사용하는 기능이다.  

```go
type Man struct {
    Person
}

var m = Man{Person{"hwangseonu", 22, "***"}}

println(m.Name)
```
*마치 Man이 Person타입을 상속 받은 것 처럼 동작한다.*

#### interface

`interface`는 메서드의 선언만을 가질 수 있다.  
메서드를 직접 구현할 수는 없으며 어떠한 `동작`을 할 수 있는 개체를 표현하기 위한 타입이다.  

```go
type Animal interface {
	Walk()
}

type Human interface {
	Animal // Animal인터페이스에 있는 모든 메서드를 포함
	Talk()
}
```

메서드는 모든 타입에 구현할 수 있으며  
특정 `interface`에서 요구하는 메서드를 올바르게 구현했다면 해당 `interface`타입으로 사용할 수 있다.  

```go
// 앞에 오는 변수는 다른 언어의 "this"와 비슷한 역할
func (변수이름 변수타입) 메서드이름(매개변수) 반환타입 {
    메서드 구현
}
```
ex) 
```go
type Dinosaur string

func (d Dinosaur) Walk() {}

var tRex Animal = Dinosaur("Tyrannosaurus rex") //Dinosaur 타입에 Walk메서드를 구현했으므로 Animal타입으로 사용할 수 있음
```

가장 많이 사용되는 경우는 역시 `struct`와 함께 사용될 때 이다.

```go
type Person struct {
    Name string // Public
    Age uint16 // Public
    secret string // Private
}

func (p Person) Walk() {}
func (p Person) Talk() {}

var h Human = Person{
    "hwangseonu", 22, "***"
} //Person 타입에 Walk, Talk메서드를 구현했으므로 Human타입으로 사용할 수 있음
```

아무 메서드도 없는 빈 `interface`의 경우  
모든 타입을 지정 할 수 있다. (Any 타입, Java의 Object와 비슷함)

```go
var a interface{} = "any type"
```
