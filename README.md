using System;
using System.Collections.Generic;
using System.Linq;

namespace AccountingApp
{
    class Program
    {
        static List<Transaction> transactions = new List<Transaction>();

        static void Main(string[] args)
        {
            Console.WriteLine("Добро пожаловать в бухгалтерскую программу!");

            while (true)
            {
                Console.WriteLine("\nВыберите действие:");
                Console.WriteLine("1. Записать транзакцию");
                Console.WriteLine("2. Просмотреть транзакции");
                Console.WriteLine("3. Показать баланс");
                Console.WriteLine("4. Выход");

                int choice = int.Parse(Console.ReadLine());

                switch (choice)
                {
                    case 1:
                        RecordTransaction();
                        break;
                    case 2:
                        ViewTransactions();
                        break;
                    case 3:
                        ShowBalance();
                        break;
                    case 4:
                        Console.WriteLine("До свидания!");
                        return;
                    default:
                        Console.WriteLine("Неверный выбор.");
                        break;
                }
            }
        }

        static void RecordTransaction()
        {
            Console.WriteLine("\nВведите тип транзакции (доход/расход):");
            string type = Console.ReadLine();

            Console.WriteLine("Введите описание транзакции:");
            string description = Console.ReadLine();

            Console.WriteLine("Введите сумму транзакции:");
            decimal amount = decimal.Parse(Console.ReadLine());

            transactions.Add(new Transaction(type, description, amount));
            Console.WriteLine("Транзакция записана.");
        }

        static void ViewTransactions()
        {
            Console.WriteLine("\nСписок транзакций:");
            if (transactions.Count == 0)
            {
                Console.WriteLine("Список транзакций пуст.");
            }
            else
            {
                foreach (Transaction transaction in transactions)
                {
                    Console.WriteLine(transaction);
                }
            }
        }

        static void ShowBalance()
        {
            decimal income = transactions.Where(t => t.Type == "доход").Sum(t => t.Amount);
            decimal expense = transactions.Where(t => t.Type == "расход").Sum(t => t.Amount);
            decimal balance = income - expense;

            Console.WriteLine("\nБаланс:");
            Console.WriteLine($"Доход: {income}");
            Console.WriteLine($"Расход: {expense}");
            Console.WriteLine($"Баланс: {balance}");
        }
    }

    // Класс для представления транзакции
    class Transaction
    {
        public string Type { get; set; }
        public string Description { get; set; }
        public decimal Amount { get; set; }

        public Transaction(string type, string description, decimal amount)
        {
            Type = type;
            Description = description;
            Amount = amount;
        }

        public override string ToString()
        {
            return $"{Type}: {Description} - {Amount}";
        }
    }
}
