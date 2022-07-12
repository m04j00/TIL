## 하이딩

- 부모 클래스와 자식 클래스 사이에 **동일한 이름**으로 멤버를 만들면 하이딩이 된다.
- 하이딩을 하면 **상속을 막을 수 있다**. 부모의 변수를 사용하기 위해서는 부모로 자료형을 변환하고 사용하면 된다.

```csharp
class Program
{
  class Parent
  {
    public int variable = 273;
  }
  class Child : Parent
  {
    public string variable = "shadowing";
  }
  static void Main(string[] args)
  {
    Child child = new Child();
    Console.WriteLine(child.variable); // shadowinig
  }
}
```
  
- 자신과 가장 가까운 것을 사용하므로 Child 객체는 클래스 내부의 문자열 변수를 사용한다.
- 부모로 취급 시 부모에 있는 메소드나 속성에 접근할 수 있다.

```csharp
static void Main(string[] args)
{
  Child child = new Child();
  Console.WriteLine((Parent)child).variable); // 273
}
```
- 메소드 하이딩은 변수와 다르게 오류가 발생하므로 표기를 해줘야 한다.

```csharp
// 하이딩 표기 
public new void Method()
{
  Console.WriteLine("자식의 메소드");
}

// 오버라이딩 표기
public override void Method()
{
  Console.WriteLine("자식의 메소드");
}
```

## 하이딩과 오버라이딩

- 부모 클래스와 자식 클래스 멤버의 이름을 동일하게 작성할 때 하이딩 또는 오버라이딩 될 수 있으므로 명확히 구분해야 한다.
- `하이딩` : 부모 클래스와 자식 클래스에서 동일한 이름을 멤버로 만들 때 발생
- `오버라이딩` : 메소드 작성 후 부모 메소드 앞에 `virtual` 키워드를 붙인다.

### new 메소드

- 하이딩 표시를 위해 메소드 이름 앞에 `new` 키워드를 붙인다.
- 하이딩은 같은 이름으로 멤버를 만들면 부모의 멤버를 숨긴다.
- 클래스 형 변환 시 숨겨진 멤버를 찾을 수 있다.

```csharp
class Child : Parent
{
  public new string variable = "hiding";
  public new void Method(){}
}
```

### virtual과 override 메소드

- 부모의 메소드에 덮어씌운다.

```csharp
class Parent
{
  public virtual void Method()
  {
    Console.WriteLine("부모의 메소드");
  }
}
class Child : Parent
{
  public override void Method()
  {
    Console.WriteLine("자식의 메소드");
  }
}
static void Main(string[] args)
{
  Child child = new Child();
  child.Method(); // 자식의 메소드
  ((Parent)child).Method(); // 자식의 메소드 
```
