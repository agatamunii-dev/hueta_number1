namespace ConsoleApp6;

class Program
{
    static void Main()
    {
        string mainS = "";
        string input = " ";
        while (input != "")
        {
            input = Console.ReadLine();
            mainS = mainS + " " + input;
        }
        
        Console.WriteLine(mainS);

        int spaces = 0;
        
        
        string[] words = mainS.Split(' ');
        
        
        

    }
}
