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
