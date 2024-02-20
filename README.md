# IncubyteAssessment
Assessment of Incubyte
using System;
using System.Collections.Generic;
using System.Linq;

public class StringCalculator
{
    public int Add(string numbers)
    {
        if (string.IsNullOrEmpty(numbers))
            return 0;

        // Default delimiters
        char[] delimiters = { ',', '\n' };

        // Check if custom delimiter is specified
        if (numbers.StartsWith("//"))
        {
            var delimiterEndIndex = numbers.IndexOf('\n');
            var customDelimiter = numbers.Substring(2, delimiterEndIndex - 2);
            delimiters = new[] { ',', '\n', customDelimiter[0] };
            numbers = numbers.Substring(delimiterEndIndex + 1);
        }

        // Split numbers by delimiters and parse to integers
        var nums = numbers.Split(delimiters).Select(int.Parse).ToList();

        // Check for negative numbers
        var negatives = nums.Where(n => n < 0).ToList();
        if (negatives.Any())
        {
            throw new ArgumentException($"Negative numbers not allowed: {string.Join(",", negatives)}");
        }

        // Sum the numbers
        return nums.Sum();
    }
}

class Program
{
    static void Main(string[] args)
    {
        var calculator = new StringCalculator();
        Console.WriteLine(calculator.Add(""));         // Output: 0
        Console.WriteLine(calculator.Add("1"));        // Output: 1
        Console.WriteLine(calculator.Add("1,5"));      // Output: 6
        Console.WriteLine(calculator.Add("1\n2,3"));   // Output: 6
        Console.WriteLine(calculator.Add("//;\n1;2")); // Output: 3
        Console.WriteLine(calculator.Add("1,\n"));     // Throws exception
    }
}
