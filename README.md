# SOILDPrinciples

# **SRP 단일 책임 원칙**  
하나의 클래스는 단 한가지의 책임만을 가져야한다.   
Class의 역할(책임)이 무엇인지 분명히 하고, 객체에서 갖는 기능(Method)만을 삽입해야 한다.

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
<hr/>
