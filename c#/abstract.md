## abstract 키워드

- 무조건 상속 또는 오버라이딩해서 사용하라는 의미이다.
- 클래스 강제 상속
    - 해당 클래스를 상속해서만 사용이 가능하다.
    - 해당 클래스 자체는 인스턴스를 만들 수 없다.
    
```csharp
abstract class Parent
{
  public void Test() {}
}
class Child : Parent
{
  public void Test() {}
}
static void Main(string[] agrs) 
{
  Parent parent = new Parent(); // 'Parent'는 추상 클래스라 인스턴스를 생성할 수 없다.
  Child child = new Child();

  parent.Test();
  child.Test();
}
```
    
- 메소드 강제 오버라이딩
    - 해당 메소드를 반드시 오버라이딩 해야 사용할 수 있다.
    - `abstract` 키워드를 메소드에 적용할 때는 반드시 클래스에도 `abstract` 키워드를 적용해야 한다.
    - abstract 메소드가 되면 중괄호를 사용하지 않고 세미콜론만 사용해 자식 클래스에서 내용을 구현하도록 한다.
    - `override`를 사용하기 위해서는 부모 메소드에 `virtual`이 붙어야 한다. 하지만 `abstract` 자체가 오버라이딩을 의미하므로 `virtual`을 함께 안붙여도 된다.
    
```csharp
abstract class Parent
{
  public abstract void Test();
}
class Child : Parent // 'Child'는 상속된 추상 멤버 'Parent.Test()'를 구현해야 한다.
{
  public override void Test() {}
}
```
