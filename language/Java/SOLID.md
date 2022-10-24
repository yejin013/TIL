# SOLID 원칙 (객체 지향 설계)

## <span style="color:red">S</span>ingle Responsibility Principle (단일 책임 원칙)

- 한 클래스는 하나의 책임만 가져야 한다.
- 클래스는 자신의 이름이 나타내는 일을 해야 한다.
- 각 개체 간의 응집력이 있다면 병합이 순 작용의 수단이 되고 결합력이 있다면 분리가 순 작용의 수단이 된다.

```java
// SRP (X)
class Animal() {
	private String species;
	public eat() {
		if ("토끼".equals(species)) {
			System.out.println(species + "는 풀을 먹는다");
		} else if ("호랑이".equals(species)) {
			System.out.println(species + "는 고기를 먹는다");
		}
	}
}
```

```java
// SRP
abstract class Animal() {
	abstract void eat();
}

class Rabbit extends Animal {
	void eat() {
		System.out.println("토끼는 풀을 먹는다");
	}
}

class Tiger extends Animal {
	void eat() {
		System.out.println("호랑이는 고기를 먹는다");
	}
}
```

## <span style="color:red">O</span>pen/Closed Principle (개방-폐쇄 원칙)

- “소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.”
- 시스템의 구조를 올바르게 리팩토링하여, 나중에 수정이 필요한 모듈을 이용하는 다른 모듈들을 줄줄이 고쳐야하는 수정 작업이 일어나지 않도록 하는 것이다.
- 기능을 추가하거나 변경해야 할 때 이미 제대로 동작하고 있던 원래 코드를 변경하지 않아도, 기존의 코드에 새로운 코드를 추가함으로써 기능의 추가나 변경이 가능하다.
- 확장에 대해 열려 있다.
    - 애플리케이션의 요구 사항이 변경될 때, 이 변경에 맞게 새로운 동작을 추가해 모듈을 확장할 수 있다.
- 수정에 대해 닫혀 있다.
    - 모듈의 소스 코드나 바이너리 코드를 수정하지 않아도 모듈의 기능을 확장하거나 변경할 수 있다.
- 핵심 요소 : 추상화
- 객체 지향 프로그래밍 언어에서 개방-폐쇄 원칙은 반드시 지켜야할 기본적인 원칙이다.
    - 유연성, 재사용성, 유지보수성을 얻게 해준다.

```java
public class Driver {

	public static void main(String[] args) {
		
		Car[] driver = new Car[2];
		driver[0] = new Truck();
		driver[1] = new Bus();
		
		for (int i = 0; i < driver.length; i++) {
			driver[i].drive();
		}
	}
}

class Car{
	public String carType = "";
	
	public void car(String carType) {
	    this.carType =carType; 
	}
	
	public void setCarType(String carType) {
	    this.carType =carType; 
	}
		
	public void drive() {
	    System.out.println(carType + " Drive");
	}
}

class Truck extends Car{
	
	public Truck() {
	    setCarType("Truck");
	}
	
	@Override
	public void drive() {
	    super.drive();
	}
}

class Bus extends Car{
	
	public Bus() {
	    setCarType("Bus");
	}	
	
	@Override
	public void drive() {
	    super.drive();
	}
	
}
// 출처: https://jeongkyun-it.tistory.com/108 [나의 과거일지:티스토리]
```

## <span style="color:red">L</span>iskov Substitution Principle (리스코프 치환 원칙)

- “프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다”
- 자식 클래스는 최소한 자신의 부모 클래스에서 가능한 행위는 수행할 수 있어야 한다.
- 상속에서 사용되는 원칙이다.

```java
// 리스코프 원칙 (X)
// 직사각형
public class Rectangle {
    private int width;
    private int height;

    public void setWidth(final int width) {
        this.width = width;
    }

    public void setHeight(final int height) {
        this.height = height;
    }

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }

    public int area () {
        return this.height * this.width;
    }
}

// 정사각형 
public class Square extends Rectangle {

    @Override
    public void setWidth(final int width) {
        super.setWidth(width);
        super.setHeight(width);
    }

    @Override
    public void setHeight(final int height) {
        super.setWidth(height);
        super.setHeight(height);
    }
}

public class Main {
    public static void main(String[] args) {
        boolean result1 = Main.checkArea(new Rectangle());
        boolean result2 = Main.checkArea(new Square());

        System.out.println(result1); // true
        System.out.println(result2); // false
    }
    
    static boolean checkArea (Rectangle rectangle) {
        rectangle.setHeight(5);
        rectangle.setWidth(4);

        return rectangle.area() == 20;
    }
}
```

```java
// 리스코프 원칙 (O)
// 도형 
public interface Shape {
    int area();
}

// 직사각형
public class Rectangle implements Shape {
    private int width;
    private int height;

    public Rectangle(int width, int height) {
        super();
        this.width = width;
        this.height = height;
    }

    @Override
    public int area () {
        return this.height * this.width;
    }
}

// 정사각형 
public class Square implements Shape {

    private int width;

    public Square(int width) {
        super();
        this.width = width;
    }

    @Override
    public int area() {
        return this.width * this.width;
    }
}

public class Main {
    public static void main(String[] args) {
        int result1 = Main.checkArea(new Rectangle(5, 4));
        int result2 = Main.checkArea(new Square(4));

        System.out.println(result1); // true
        System.out.println(result2); // false
    }

    static int checkArea (Shape shape) {
        return shape.area();
    }
}
```

## <span style="color:red">I</span>nterface Segregation Principle (인터페이스 분리 원칙)

- “특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다”
- 클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 한다.

```java
interface Playable {
    void play();
}

interface Watchable {
    void watch();
}

interface Calculatable {
    void calculate();
}

class MP3 implements Playable {
    // 음악 재생만 
}

class PMP implements Playable, Watchable {
    // 음악 재생과 영상 재생 
}

class Phone implements Playable, Watchable, Calculatable {
    // 모든 기능 가능
}
```
## <span style="color:red">D</span>ependency Inversion Principle (의존관계 역전 원칙)

- 프로그래머는 “추상화에 의존해야지, 구체화에 의존하면 안된다.”
- 상위 계층이 하위 계층에 의존하는 전통적인 의존관계를 반전시킴으로써 상위 계층이 하위 계층의 구현으로부터 독립되게 할 수 있다.
- 상위 모듈은 하위 모듈에 의존해서는 안 된다.
- 추상화는 하위 계층에 의존해서는 안 된다.

```java
public class Main {
    public static void main(String[] args) {
        Connectable mySQLConnection = new MySQLConnection();
        Connectable h2Connection = new H2Connection();
        
        DBConnection dbConnection = new DBConnection(mySQLConnection);
        dbConnection.getDbConnection();
    }
}

public class DBConnection {
    private Connectable dbConnection;
    
    public DBConnection(Connectable dbConnection) {
        this.setDbConnection(dbConnection);
        dbConnection.connect();
    }

    public Connectable getDbConnection() {
      return dbConnection;
    }

    public void setDbConnection(Connectable dbConnection) {
      this.dbConnection = dbConnection;
    }
}

public interface Connectable() {
    void connect();    
}

public class MySQLConnection implements Connectable {
    @Override
    void connect() {
        // mysql db 연결
    }
}

public class H2Connection implements Connectable {
  @Override
  void connect() {
    // mysql db 연결
  }
}
```


참고: [https://ko.wikipedia.org/wiki/SOLID_(객체_지향_설계)](https://ko.wikipedia.org/wiki/SOLID_(%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%EC%84%A4%EA%B3%84)), [https://www.nextree.co.kr/p6960/](https://www.nextree.co.kr/p6960/)