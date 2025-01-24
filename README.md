# Conversii.
using System;
using System.Text;

class Program
{
    static void Main(string[] args)
    {
        // 1. Citirea inputului
        Console.Write("Introduceți numărul (în baza b1): ");
        string inputNumber = Console.ReadLine();

        Console.Write("Introduceți baza inițială (b1, între 2 și 16): ");
        int b1 = int.Parse(Console.ReadLine());
        
        Console.Write("Introduceți baza finală (b2, între 2 și 16): ");
        int b2 = int.Parse(Console.ReadLine());

        // Validare baze
        if (b1 < 2 || b1 > 16 || b2 < 2 || b2 > 16)
        {
            Console.WriteLine("Bazele trebuie să fie între 2 și 16.");
            return;
        }

        // 2. Conversia din baza b1 în baza 10
        string[] parts = inputNumber.Split('.');
        string integerPart = parts[0];
        string fractionalPart = parts.Length > 1 ? parts[1] : "";

        double decimalNumber = ConvertToDecimal(integerPart, fractionalPart, b1);

        // 3. Conversia din baza 10 în baza b2
        string result = ConvertFromDecimal(decimalNumber, b2);

        // 4. Afișarea rezultatului
        Console.WriteLine($"Numărul {inputNumber} în baza {b1} este {result} în baza {b2}.");
    }

    // Funcție care convertește un număr din baza b1 în baza 10
    static double ConvertToDecimal(string integerPart, string fractionalPart, int base1)
    {
        double result = 0;

        // Partea întreagă
        for (int i = 0; i < integerPart.Length; i++)
        {
            int digitValue = GetDigitValue(integerPart[i]);
            result = result * base1 + digitValue;
        }

        // Partea fracționară
        double fractionalMultiplier = 1.0 / base1;
        for (int i = 0; i < fractionalPart.Length; i++)
        {
            int digitValue = GetDigitValue(fractionalPart[i]);
            result += digitValue * fractionalMultiplier;
            fractionalMultiplier /= base1;
        }

        return result;
    }

    // Funcție care convertește un număr din baza 10 în baza b2
    static string ConvertFromDecimal(double decimalNumber, int base2)
    {
        StringBuilder result = new StringBuilder();

        // Partea întreagă
        long integerPart = (long)Math.Floor(decimalNumber);
        double fractionalPart = decimalNumber - integerPart;

        // Diviziuni succesive pentru partea întreagă
        do
        {
            result.Insert(0, GetDigitCharacter((int)(integerPart % base2)));
            integerPart /= base2;
        } while (integerPart > 0);

        // Adăugăm partea fracționară
        if (fractionalPart > 0)
        {
            result.Append('.');
            int precision = 10; // Controlăm câte zecimale să calculăm
            while (fractionalPart > 0 && precision-- > 0)
            {
                fractionalPart *= base2;
                int digit = (int)Math.Floor(fractionalPart);
                result.Append(GetDigitCharacter(digit));
                fractionalPart -= digit;
            }
        }

        return result.ToString();
    }

    // Funcție pentru a obține valoarea numerică a unui caracter (0-9, A-F)
    static int GetDigitValue(char digit)
    {
        if (char.IsDigit(digit))
            return digit - '0';
        else
            return char.ToUpper(digit) - 'A' + 10;
    }

    // Funcție pentru a obține caracterul corespunzător unei valori (0-9, A-F)
    static char GetDigitCharacter(int value)
    {
        if (value < 10)
            return (char)('0' + value);
        else
            return (char)('A' + (value - 10));
    }
}
