## Sealed 키워드

- 클래스와 메소드 앞에 적용하는 키워드
- 클래스 앞 : 클래스 상속 제한

```csharp
sealed*class Parent
{
  public void Test() {} 
}
class Child : Parent // sealed 형식'Parent'에서 파생될 수 없다. 
{
  public void Test() {}
}
static void Main(string[] args)
{
  Parent parent = new Parent();
  Child child = new Child();
  parent.Test();
  parent.Test();
}
```
    
- 메소드 앞 : 메소드 오버라이딩 제한
    
```csharp
class Parent
{
  public virtual void Test() {}
}
class Child : Parent
{
  sealed public override void Test() {}
}
class GrandChild : Child
{
  // 'GrandChild.Test()' : 상속된 'Child.Test()' 멤버가 sealed라 재정의할 수 없음 
  public override void Test() {} 
}
```
