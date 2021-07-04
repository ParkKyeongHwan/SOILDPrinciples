# SOILDPrinciples

## **SRP 단일 책임 원칙**  
하나의 클래스는 단 한가지의 책임만을 가져야한다.   
Class의 역할(책임)이 무엇인지 분명히 하고, 객체에서 갖는 기능(Method)만을 삽입해야 한다.

### Bad Way
```C#
public class Parser
{
	public string path {get; set;}
	public List<string>lines {get; set;}
	public void Parse()
	{
		StreamReader streamReader = StreamReader(path;)
		while(!reader.EndOfStream)
		{
			lines.Add(reader.ReadLine());
		}

		string splitCSV = string.Empty;
		foreach(string line in lines)
		{
			splitCSV = line.split(',');
		}

		for(int index =0; spltCSV.Length; index++)
		{
			if(splitCSV[index].Eqquals("parsedData"))
			{
				// Do your Parse..
			}
		}
	}
}
```

### Good Way
```C#
public class EquipmentInfoParser
{
	public CommaSeparatedValues CSV ( get; private set; )
	public EquipmentInfoParser(CSV csv)
	{
		this.CSV = csv;
	}
	
	public void Parse()
	{
		for(int index = 0; CSV.Length; index++)
		{
			if(CSV.Row[index].Equals("parsedData"))
			{
				// Do your Algorithm of parser..
			}
		}
	}
}

public interface IFileReader
{
	List<string> Lines { get; }
}

public class TextFileReader : IFileReader
{
	public string Path { get; private set; }
	public List<string> Lines { get; private set; }
	public void ReadFile(string path)
	{
		this.Parh = path;
		StreamReader streamReader = StreamReader(this.Path);
		while(!reader.EndOfStream)
		{
			Lines.Add(reader.ReadLine());
		}
	}
}


public class CommaSeparatedValues
{
	private IFileReader fileReader = null;
	public Rows row { get; private set; } // 생략
	public Columns column { get; private set; } // 생략
	public CommaSeparatedValues(IFileReader fileReader)
	{
		this.fileReader = fileReader;
	}
	public void ParseToCSV()
	{
		foreach(string line in this.Lines)
		{
			// Do line.split(',');
		}
	}
}
```


### 단일책임원칙은 정답이 존재하지 않는가? : **그렇지 않다.**   
#### 1) 모든 개발자는 Class가 어떤 행위를 책임질 것인지 반드시 정의하고 개발합니다.   
단지 어떤 행위를 수행할지 정하였으나 이탈되어 위배했거나 Class 이름 자체를 추상적으로 정의했기 때문입니다.   
추상적인 Class는 추상적인 행위만 실행해야하고, 구체적인 Class는 구체적인 실행을 합니다.   
추상적인 Class를 다시 시간, 공간, 물질에 한정시켜보면 구체적으로 변합니다.   
이름 자체가 추상화가 된다면 이를 구체화해야합니다.   

예를 들어 DataAnalyzer Class라면 Data가 정확히 무엇이고 분석이라고 하지 않고   
값을 솎아내는 행위를 하는지, 값을 계산하는지, 값을 정렬하는지, 값을 Has하는지를 명확히 구분해줍니다.
   
구체적이지 않은 클래스의 예시)   
class DataAnalyzer, class Algorithm, class Commander, class ProgramName, ..   

#### 2) 그래도 잘 모르겠다면, 이 땐 내부에 구현한 '행위'를 살펴보세요!
<hr/>

## **OCP 개방폐쇄원칙**
객체는 기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계되어야 한다.   
객체에서 한 군데를 변경한 것이 의존적인 모듈에서 단계적인 변경을 일으킬 때, 이 설계는 경직성에서 악취를 풍긴다.   
OCP를 보는 또 하나의 관점은 클래스를 변경하지 않고도 대상 클래스의 환경을 변경할 수 있는 설계가 되어야 한다는 것이다.   
이것은 특히 단위테스트를 수행할 때 매우 중요하다.   

> 열린 확장 : 모듈의 행위가 쉽게 추가될 수 있고, 요구사항 변경 시, 요구사항에 맞게 새로운 행위를 추가 개발할 수 있도록 해야한다.   
> 닫힌 수정 : 어떤 행위를 확장하는 것이 그 모듈의 소스코드나 바이너리 코드의 변경을 초래하지 않는다.   

어떻게 모듈의 소스코드를 변경하지 않은 채로 그 모듈이 하는 일을 변경할 수 있을까?   

해결책은 추상화다.   

> 그래디 부치(Grady Booch)에 의하면 '추상화란 다른 모든 종류의 객체로부터 식별될 수 있는 객체의 본질적인 특징'이라고 정의한다.   
  
설계방법   
1. 변경(확장)될 것과 변하지 않을 것을 구분한다.   

2. 이 두 모듈이 만나는 지점에 인터페이스를 정의한다.   

3. 구현의 의존하기보다는 정의한 인터페이스에 의존하도록 코드를 작성한다.   

### Bad Way
``` C#
Enum OrderType
{
	Shuffle, Reverse, ...
}

public class DataGenerator
{

	private IList<int> list = null;
	public DataGenerator(IList<int> list)
	{
		this.list = list;
	}

	public List<int> GenerateTo(OrderType orderType)
	{
		if(orderType is OrderType.Shuffle)
		{
					
		}
		else if(orderType is OrderType.Reverse)
		{
					
		}
		else if(만약 OrderType이 추가된다면...?) {...}
	}
}
```

``` C#
public interface IStrategy
{
	List<int> Run(List<int> numbers);
}

public class ShufflStrategy : IStrategy
{
	public List<int> Run(List<int> numbers)
	{
		// Do Shuffle..
	}
}

public class ReverseStrategy : IStrategy
{
	public List<int> Run(List<int> numbers)
	{
		// Do Reverse..
	}
}

// 만약 RandomStrategy가 추가되어도 GenerateTo(IStrategy)는 변경되지 않는다.
// 이 때 GenerateTo를 기능을 추가할 수 있는 확장이 열려있다고 하고
// 기능(객체)이 추가되더라도 변경되지 않는 코드의 폐쇄를 따른다고 한다.
public class DataGenerator
{
	public List<int> GenerateTo(IStrategy strategy)
	{
		var iWantResult = strategy.Run();
		// Do..
		return iWantResult;
	}
}
```
<hr/>

## **LSP 리스코프 치환 원칙**   

**리스코프 치환 원칙이란, 서브 타입은 언제나 자신의 기반 타입으로 교체할 수 있어야 한다는 의미입니다.**   
상위 객체는 하위 객체의 교체를 쉽게 하고, 기능 변경 시 중단되지 않아야 하며 일부 기능만 손실되어야 합니다.   
객체 지향의 상속은 다음의 조건을 만족해야합니다.   

> 하위클래스 is a kind of 상위클래스 : 하위 분류는 상위 분류의 한 종류다.   

> 구현클래스 is able to 인터페이스 : 구현 분류는 인터페이스할 수 있어야 한다.   
   
**어떻게 보자면, 상속될 부모클래스는 대명사의 개념으로 이해하면 좋다.**
   
### Example   
``` C#
public class abstract Logger
{
	public abstract void WriteLog() {}
}

public class FileLogger : Logger {} // FileLogger는 Logger의 한 종류다.
public class DataBaseLogger : Logger {} // DataBaseLogger는 Logger의 한 종류다.
public class XMLLogger : Logger {} // XMLLogger는 Logger의 한 종류다.
public class TextFormatLogger : Logger {} // TextFormatLogger는 Logger의 한 종류다.
```

### Good Way
``` C#
class Animal
{
	virtual Eat() 
}
class Human : Animal // 정상
{
	override Eat()
}
class Rabbit : Animal // 정상
{
	override Eat()
}
```   
   
개발자가 많은 실수를 하는 부분은 **상속을 코드의 재사용이라고 오해하는 것**이다.
   
### Bad Way
``` C#
class Human
{
	virtual Eat() 
}
class Animal : Human
{
	override Eat() // 재사용해야지.. (잘못)
}
class Rabbit : Animal // 토까는 동물이며, 사람이기도 하다가 성립됨 (잘못)
{
	override Eat() // 재사용해야지.. (잘못)
}

// 어쩌면 위 코드에서도 문제점을 발견하지 못할 수 있는데
// 진짜 문제는 아래 코드에서 발생된다.
public class Action
{
	Human human = null;
	public Action(Human human)
	{
		this.human = human;
	}
	public void Eat()
	{
		this.human.Eat(); // 사람의 먹는다를 수행.
	}
}

// Usage
Action action = new Action(new Rabbit());
action.Eat(); // 토끼의 먹는다를 수행하지만, Action에서는 사람의 먹는다로 코드를 오용함.
```

**왜 이런 모호한 일이 발생하는가? : 유효성은 본래 갖추어진 것이 아니다.**

흔한 예로, 직사각형은 정사각형이 아니라고 할 수는 있지만, 정사각형은 직사각형이라고 할 수 있다는 점에서 발생된다.

자체 모순이 없는 설계라고 해서 반드시 모든 고객(객체 또는 사용자)에 대해서 모순이 없는 것은 아니다.

이러한 문제는 모델만 보고 그 모델의 유효성을 충분히 검증할 수 없기 때문이다.

어떤 모델의 유효성은 오직 고객(사용 객체) 관점으로만 표현된다.

아래는 정사각형이 직사각형의 기능을 상속받는 예제입니다.

### Example
``` C#
class Rectangle
{
	protected decimal _width;
	protected decimal _heigtht;
	public virtual void SetWidth(decimal w) { // ... }
	public virtual void SetHeight(decimal h) { // ... }
}

class Square : Rectnangle
{
	public override void SetWidth(decimal w)
	{
		_width = w;
		_height = w;
	}
	public override void SetHeight(decimal h)
	{
		_width = h;
		_height = h;
	}
}

public void GetArea(Rectangle rect)
{
	rect.SetWidth(5);
	rect.SetHeight(4);
	assert(rect.Area() == 20);
}

// Usage
Call GetArea(new Rectangle()); // width:5 x height:4 == 20
Call GetArea(new Square()); // width:4 x height:4 == 16 Failure!!
```
<hr/>

## **ISP 인터페이스 분리 원칙**

**인터페이스는 자신이 사용하지 않는 메서드에 의존되도록 강제되어서는 안된다.**

ISP 분리원칙이란, 인터페이스는 이용하지 않을 메서드에 의존되지 않아야 한다는 원칙입니다.

인터페이스들은 구체적이고 작은 단위들로 분리시킴으로서 Client(객체 또는 함수)가 꼭 필요한 메서드로만 이용될 수 있도록 하는 것에 목적을 두고 있습니다.

### Bad Way
``` C#
public interface IFileReader
{
	void OpenFile();
}
public interface IFileManager
{
	void OpenFile();
	void CreateFile();
	bool CanAccessDirectory { get; };
}
```

### Good Way
``` C#
public interface IFileReader
{
	void OpenFile();
}
public interface IFileManager : IFileReader
{
	void CreateFile();
	bool CanAccessDirectory { get; };
}
```
<hr/>

## **DIP 의존 역전 원칙**

**자주 변화하는 것보다 변화하기 어렵거나 변화가 없는 것에 의존되어야 한다.**

DIP 의존 역전 원칙이란, 상위 수준의 모듈은 하위 수준의 모듈에 의존해서는 안된다.

추상화는 구체적인 사항에 의존해서는 안된다. 구체적인 사항은 추상화에 의존되어야 한다.

사람의 이름, 성별, 나이 등은 그 자체의 속성으로서 변화하기가 어렵지만 상대적으로 자동차, 핸드폰, 옷은 교체될 수 있다.

정책, 전략과 같은 큰 거시적인 흐름 같은 추상적인 것은 변화하기 어렵고 구체적인 방식이나 사물 등은 변하기 쉬운 것으로 구분하면 좋다.

> 인터페이스, 추상클래스 : 변하지 않는 것.

> 구체적인 클래스 : 변하는 것.

   
   
### Bad Way
``` C#
public class Kid
{
	private Lego lego = null;
	public void SetToy(Lego lego)
	{
		this.lego = lego;
	}
	public void PlayWithLego()
	{
	}
}
```


혹시나 요구사항이 변경되거나 추가되어 Robot이 추가되면..

```csharp
public class Kid
{
	private Lego lego = null;
	private Robot robot = null;
	public void SetToy(Lego lego)
	{
		this.lthis.lego = lego;
	}
	public void SetToy(Robot robot)
	{
		this.robot = robot;
	}

	public void PlayWithLego()
	{
		// Play..
	}

	public void PlayWithRobot()
	{
		// Play..
	}
}
```

### Good Way
``` C#
public class Kid
{
	private Toy toy = null;
	public void SetToy(Toy toy)
	{
		this.toy = toy;
	}
	public void Play()
	{
		// MidleMan(Smell Code)이지만 가벼운 예제로 넘어가자
		// play with toy..
	}
}

public abstract class Toy
{
	public abstract void Play();
}

public class Lego :Toy
{
	public override void Play()
}

public class Robot : Toy
{
	public override void Play();
}
```
