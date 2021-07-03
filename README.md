# SOILDPrinciples

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
