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
        whie(!reader.EndOfStream)
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
