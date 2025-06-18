P_W01 Explain problems in the following code

<pre lang="csharp">
using CodeProblems;

var person = new PersonName("Maria", "Curie", "Skłodowska");
foreach (var part in person)
    Console.WriteLine(part.ToUpper());

var logger = new FileLogger("log.txt");
logger.Log("Program started");
logger.Log("Processing data...");

var values = new List<int> { 1, 2, 3, 4, 5 };
foreach (var value in values)
{
    if (value % 2 == 0)
        values.Remove(value);
}

namespace CodeProblems
{
    public class PersonName : IEnumerable<string>
    {
        public string FirstName { get; }
        public string LastName { get; }
        public string MiddleName { get; }

        public PersonName(string first, string last, string middle = null)
        {
            FirstName = first;
            LastName = last;
            MiddleName = middle;
        }

        public IEnumerator<string> GetEnumerator()
        {
            yield return FirstName;
            yield return MiddleName;
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
            => GetEnumerator();
    }

    public class FileLogger : IDisposable
    {
        private readonly StreamWriter _writer;

        public FileLogger(string path) => _writer = new StreamWriter(path);

        public void Log(string message) => _writer.WriteLine($"{DateTime.Now}: {message}");

        public void Dispose()
        {

        }
    }
}
</pre>

**Your answers:**
1. using System.Collections should be added to the code or use System.Collections.IEnumerable, but it's a bad implementation
2. In the foreach loop, part is an anonymous Type, it's not a string and method ToUpper() cannot be used. To fix it IEnumerator should be generic<string>
3. var logger = new FileLogger("log.txt"); it should be used with the using keyword, Dispose will delete the logger after using 
4.logger.Log("Processing data..."); all this stuff should be in using(var logger = new FileLogger("log.txt");){logger.Log("Program started");   logger.Log("Processing data...");}
logger.Log("Processing data...");
5. string MiddleName should be string? 
6. It's really strange that GetEnumerator does not return a LastName
7. MiddleName should be checked for potential null in GetEnumerator
8. System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()  => GetEnumerator();                                                                                    Yeah it will be work, but it can be shorter by deleating System.Collections and using using System.Collections;
9. void Dispose cannot be empty, at least GC.SomeKindaMethodthatDelete(this), also there can be added protected Dispose(bool)
10. List should be generic because List is from System.Collection.Generic. List<int>
11. foreach (var value in values) should be change to Linq expression
