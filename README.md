namespace ConsoleApp2;
class Program
{
    static string GetInputText() //тут просто повертає один рядок куди ми обєднали усі
    {
        string mainS = "";
        string input = " ";
        while (input != "")
        {
            input = Console.ReadLine();
            mainS += " " + input;
        }
        //увага! просто рядок
        return mainS; //поветрає стрінг, в оголощенні функції тип як раз стрінг
    }

    static Dictionary<string, int> GetDictionary(string mainS)//повертає словник, приймає строку. зробила окремо бо мб знадобиться
    {
        string[] words = mainS.Split(new[] { ' ', '.', ',', '!', '?' }, StringSplitOptions.RemoveEmptyEntries);
        //зверху імба, із рядка залишаються лише слова і нічого більше

        Dictionary<string, int> dict = new Dictionary<string, int>();
        foreach (string word in words)
        {
            string lowWord = word.ToLower(); //робить слово з малої
            if (dict.ContainsKey(lowWord))
            {
                dict[lowWord]++;
            }
            else
            {
                dict[lowWord] = 1;
            }
        }
        return dict;
    }

    //тут далі йдуть функції по таскам
    static void Task1(string mainS) //до завдання1, нічо не дає тому void, бере в себе меінС як стрінг
    {
        int countOfSpaces = 0;
        int countOfWords = 0;
        int countOfSentence = 0;
        for (int i = 0; i < mainS.Length; i++)
        {
            if (mainS[i] == ' ')
            {
                countOfSpaces++;
            }
            if (mainS[i] == '.')
            {
                countOfSentence++;
            }
        }//далі тупо аналіз шо ксть пробілів == ксть слів-1, ксть точок == ксть речень і т.д
        Console.WriteLine("");
        Console.WriteLine("TASK 1");
        Console.WriteLine($"Count of sentense: {countOfSentence}");
        Console.WriteLine($"Count of words:, {countOfSpaces - 1}");
        Console.WriteLine($"Count of symbols:  {mainS.Length - countOfSpaces}");
    }

    static void Task2(Dictionary<string, int> dict)
    {
        Console.WriteLine("");
        Console.WriteLine("TASK 2");
        int countOfDict = dict.Count;
        if (countOfDict < 5)
        {
            foreach (var pair in dict)
            {
                Console.WriteLine($"Most popular word: {pair.Key} - {pair.Value} times");
            }
            return;//нічо не повертає просто з функції виходить
        }

        string[] words = new string[countOfDict];//дістає із словника ключ і кладе в массив
        int[] counts = new int[countOfDict];//тут так само але із значеннями

        int head = 0;
        foreach (var pair in dict)
        {
            words[head] = pair.Key;
            counts[head] = pair.Value;
            head++;
        }

        int max = 0;
        int indexOfMax = -1;
        for (int i = 0; i < 5; i++)//тупо знаходить максимум і його індекс, виводить слово і занулює теперішній максимум щоб не заважав
        {
            max = counts.Max();
            indexOfMax = Array.IndexOf(counts, max);
            Console.WriteLine($"{i + 1}st most popular word: {words[indexOfMax]} - {max} times");
            counts[indexOfMax] = 0;
        }
        return;
    }

    static void Task3(Dictionary<string, int> dict) //нічо не повертає, бере dict
    {
        float totalLength = 0;//загальна ксть букв
        float countOfWords = 0;//на шо ділити загальну ксть букв щоб вийшло середнє
        foreach (var pair in dict)
        {
            totalLength += pair.Key.Length;
            countOfWords++;
        }
        Console.WriteLine("");
        Console.WriteLine("TASK 3");
        Console.WriteLine($"Average word lenght: {totalLength / countOfWords}");
    }

    // ------------------ ДАЛІ СТУДЕНТ Б (пп. 4–6) ------------------

    static List<string> GetSentences(string mainS) //просто ділю на речення. дуже просте
    {
        List<string> sentences = new List<string>();
        string current = "";

        for (int i = 0; i < mainS.Length; i++)
        {
            current += mainS[i];

            // кінець речення
            if (mainS[i] == '.' || mainS[i] == '!' || mainS[i] == '?')
            {
                string s = current.Trim();
                if (s != "")
                    sentences.Add(s);
                current = "";
            }
        }

        // якщо текст без точки в кінці, то теж додаю
        string last = current.Trim();
        if (last != "")
            sentences.Add(last);

        return sentences;
    }

    static int CountWordsInSentence(string sentence) //рахую слова в реченні (по пробілах і знаках)
    {
        string[] w = sentence.Split(new[] { ' ', '.', ',', '!', '?', ':', ';', '-', '\t', '\n', '\r' },
            StringSplitOptions.RemoveEmptyEntries);
        return w.Length;
    }

    static void Task4(string mainS) //найдовше та найкоротше речення (по кількості слів)
    {
        Console.WriteLine("");
        Console.WriteLine("TASK 4");

        List<string> sentences = GetSentences(mainS);

        if (sentences.Count == 0)
        {
            Console.WriteLine("No sentences");
            return;
        }

        int minWords = int.MaxValue;
        int maxWords = -1;
        string shortest = "";
        string longest = "";

        foreach (string s in sentences)
        {
            int c = CountWordsInSentence(s);

            if (c < minWords)
            {
                minWords = c;
                shortest = s;
            }

            if (c > maxWords)
            {
                maxWords = c;
                longest = s;
            }
        }

        Console.WriteLine($"Shortest sentence ({minWords} words): {shortest}");
        Console.WriteLine($"Longest sentence ({maxWords} words): {longest}");
    }

    static void Task5(ref string mainS) //пошук та заміна слова
    {
        Console.WriteLine("");
        Console.WriteLine("TASK 5");

        Console.Write("Enter word to find: ");
        string findW = Console.ReadLine();
        Console.Write("Enter word to replace: ");
        string replW = Console.ReadLine();

        if (string.IsNullOrEmpty(findW))
        {
            Console.WriteLine("Empty find word");
            return;
        }

        // дуже тупо але працює: ділю по пробілах, замінюю якщо слово співпало (без регістру)
        string[] parts = mainS.Split(' ');

        for (int i = 0; i < parts.Length; i++)
        {
            string token = parts[i];

            // прибираю пунктуацію по краях щоб порівняти
            string core = token.Trim('.', ',', '!', '?', ':', ';', '"', '\'', '(', ')', '[', ']');

            if (core.Equals(findW, StringComparison.OrdinalIgnoreCase))
            {
                // зберігаю пунктуацію як була (дуже примітивно)
                string left = "";
                string right = "";

                int start = 0;
                while (start < token.Length && (token[start] == '.' || token[start] == ',' || token[start] == '!' || token[start] == '?' ||
                                               token[start] == ':' || token[start] == ';' || token[start] == '"' || token[start] == '\'' ||
                                               token[start] == '(' || token[start] == ')' || token[start] == '[' || token[start] == ']'))
                {
                    left += token[start];
                    start++;
                }

                int end = token.Length - 1;
                while (end >= 0 && (token[end] == '.' || token[end] == ',' || token[end] == '!' || token[end] == '?' ||
                                    token[end] == ':' || token[end] == ';' || token[end] == '"' || token[end] == '\'' ||
                                    token[end] == '(' || token[end] == ')' || token[end] == '[' || token[end] == ']'))
                {
                    right = token[end] + right;
                    end--;
                }

                parts[i] = left + replW + right;
            }
        }

        mainS = string.Join(" ", parts);
        Console.WriteLine("Done replace");
    }

    static void Task6(string mainS) //вивід тексту у зворотному порядку слів у кожному реченні
    {
        Console.WriteLine("");
        Console.WriteLine("TASK 6");

        List<string> sentences = GetSentences(mainS);

        if (sentences.Count == 0)
        {
            Console.WriteLine("No sentences");
            return;
        }

        // просто друкую кожне речення але слова навпаки
        for (int i = 0; i < sentences.Count; i++)
        {
            string s = sentences[i].Trim();

            // забираю знак в кінці щоб не заважав
            char endChar = '\0';
            if (s.Length > 0)
            {
                char last = s[s.Length - 1];
                if (last == '.' || last == '!' || last == '?')
                {
                    endChar = last;
                    s = s.Substring(0, s.Length - 1);
                }
            }

            string[] w = s.Split(new[] { ' ', '\t', '\n', '\r' }, StringSplitOptions.RemoveEmptyEntries);
            Array.Reverse(w);

            string outS = string.Join(" ", w);
            if (endChar != '\0')
                outS += endChar;

            Console.WriteLine(outS);
        }
    }

    static void Main()
    {
        string mainS = GetInputText(); // тут у маінС строка, зчитуємо ж лише один раз
        Dictionary<string, int> dict = GetDictionary(mainS); //на всяк випадок окремо зробила словник, мб тобі знадобиться

        Task1(mainS);
        Task2(dict);
        Task3(dict);

        Task4(mainS);        // п.4
        Task5(ref mainS);    // п.5 (тут міняю mainS)
        Task6(mainS);        // п.6 (після заміни)

        Console.WriteLine("");
        Console.WriteLine("TEXT AFTER REPLACE:");
        Console.WriteLine(mainS);
    }
}

