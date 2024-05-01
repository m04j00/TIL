# Sealed class
- Sealed 클래스는 클래스 **계층 구조를 정의**하는 특별한 종류의 클래스다.
- sealed 클래스는 상속의 개념이 아니라 **클래스의 종류를 제한하는 용도**로 사용된다.
- sealed 클래스는 해당 클래스의 하위 클래스를 한정하는 데 사용되며, 이 **하위 클래스들은 sealed 클래스의 내부나 동일한 파일 내에서 정의**되어야 한다. 이렇게 하면 sealed 클래스의 하위 클래스가 완전하고 제한된 집합을 형성하게 되어, 해당 클래스 계층 구조를 제한하고 더욱 안정적으로 만든다.

1. **제한된 하위 클래스**: Sealed class는 제한된 수의 하위 클래스를 가질 수 있다. 
2. **패턴 매칭**: Sealed class는 패턴 매칭과 함께 사용되어 특정 유형의 인스턴스를 처리하는 코드를 보다 간결하게 작성할 수 있다. `when` 표현식을 사용하여 sealed class의 각 하위 클래스에 대해 다른 동작을 수행할 때 유용하다.
3. **캡슐화된 유형 계층**: Sealed class를 사용하면 유형의 계층 구조를 명확하게 정의할 수 있다. 이는 코드의 가독성과 유지 보수성을 향상시키고, 클래스 간의 관계를 명확하게 보여준다.

```kotlin
sealed class Character {
    class Warrior(val name: String, val weapon: String) : Character()
    class Mage(val name: String, val spell: String) : Character()
    class Rogue(val name: String, val dagger: String) : Character()
}

fun attack(character: Character) {
    when (character) {
        is Character.Warrior -> println("${character.name} swings ${character.weapon}.")
        is Character.Mage -> println("${character.name} casts ${character.spell}.")
        is Character.Rogue -> println("${character.name} backstabs with ${character.dagger}.")
    }
}

fun main() {
    val warrior = Character.Warrior("Conan", "Sword")
    val mage = Character.Mage("Merlin", "Fireball")
    val rogue = Character.Rogue("Thief", "Dagger")

    attack(warrior)
    attack(mage)
    attack(rogue)
}
```

## 하위 Class와 Object

### 정의

- **파일 내에 하위 클래스로 정의**한다. Sealed 클래스와 동일한 파일에서 하위 클래스를 정의하고 외부에서 사용하면 코드의 가독성과 유연성이 향상된다.
- Sealed 클래스 내부에 클래스 정의는 피해야 한다. Sealed 클래스는 상위 수준의 클래스 계층을 제한하는 용도로 사용된다. 내부에 클래스를 정의하는 것은 유연성을 제한하고 가독성을 해칠 수 있다.

Result.kt

```kotlin
sealed class Result {
    companion object {
        fun success(message: String): Result = Success(message)
        fun error(error: Throwable): Result = Error(error)
    }
}

data class Success(val message: String) : Result()
data class Error(val error: Throwable) : Result()
```

main.kt

```kotlin
fun main() {
    val successResult = parseResult("Success: Data loaded successfully")
    val errorResult = parseResult("Error: Data loading failed")
    
    when (successResult) {
        is Success -> println("Success: ${successResult.message}")
        is Error -> println("Error: ${successResult.error.message}")
    }
}

fun parseResult(resultString: String): Result {
    return if (resultString.startsWith("Success")) {
        val message = resultString.substringAfter(": ")
        Result.success(message)
    } else {
        Result.error(Exception("Parsing failed"))
    }
}
```

### Class

- sealed 클래스의 특정한 종류를 나타내며, 해당 클래스의 속성을 확장하거나 변경할 수 있다.
- 하위 클래스는 sealed 클래스의 특징을 상속하며, 추가적인 기능을 제공한다.
- 예를 들어, 다양한 유형의 데이터를 처리하는 경우 sealed 클래스의 하위 클래스로 각 유형을 나타내는 클래스를 정의할 수 있다.

```kotlin
sealed class Vehicle {
    class Car(val model: String) : Vehicle()
    class Motorcycle(val model: String) : Vehicle()
}
```

### Object

- sealed 클래스의 특정한 인스턴스를 나타내는 싱글톤 객체를 정의할 때 사용된다.
- 객체는 해당 클래스의 유일한 인스턴스를 나타내며, 싱글톤 패턴을 구현할 때 유용하다.
- 예를 들어, 애플리케이션의 상태를 관리하는 데 사용되는 sealed 클래스의 객체를 정의할 수 있다.

```kotlin
sealed class Vehicle {
    object Car : Vehicle()
    object Motorcycle : Vehicle()
}
```

---
- [참고](https://gold.gitbook.io/kotlin/class/sealed)
