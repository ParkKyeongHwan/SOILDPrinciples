# SOILDPrinciples

## **SRP 단일 책임 원칙**  
하나의 클래스는 단 한가지의 책임만을 가져야한다.   
Class의 역할(책임)이 무엇인지 분명히 하고, 객체에서 갖는 기능(Method)만을 삽입해야 한다.

### Bad Way
<pre><code>
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
</code></pre>

### Good Way
<pre><code>
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
</code></pre>
<hr/>

### 단일책임원칙은 정답이 존재하지 않는가?
#### **그렇지 않다.**
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
