using System;
using System.Collections.Generic;

namespace ATMApplication
{
    public class Account
    {
        public int AccountNumber { get; set; }
        public double Balance { get; private set; }
        public double InterestRate { get; set; }
        public string AccountHolderName { get; set; }
        private List<string> transactions;

        public Account(int accountNumber, double initialBalance, double interestRate, string accountHolderName)
        {
            AccountNumber = accountNumber;
            Balance = initialBalance;
            InterestRate = interestRate;
            AccountHolderName = accountHolderName;
            transactions = new List<string>();
        }

        public void Deposit(double amount)
        {
            if (amount > 0)
            {
                Balance += amount;
                transactions.Add($"Deposited: {amount:C}");
            }
            else
            {
                Console.WriteLine("Deposit amount must be greater than zero.");
            }
        }

        public void Withdraw(double amount)
        {
            if (amount > 0 && amount <= Balance)
            {
                Balance -= amount;
                transactions.Add($"Withdrew: {amount:C}");
            }
            else
            {
                Console.WriteLine("Withdrawal amount must be greater than zero and less than or equal to the balance.");
            }
        }

        public void DisplayTransactions()
        {
            Console.WriteLine("Transaction History:");
            foreach (var transaction in transactions)
            {
                Console.WriteLine(transaction);
            }
        }
    }
}


namespace ATMApplication
{
    public class Bank
    {
        private List<Account> accounts;

        public Bank()
        {
            accounts = new List<Account>();
            for (int i = 0; i < 10; i++)
            {
                accounts.Add(new Account(100 + i, 100, 0.03, $"AccountHolder {100 + i}"));
            }
        }

        public void AddAccount(Account account)
        {
            accounts.Add(account);
        }

        public Account RetrieveAccount(int accountNumber)
        {
            foreach (var account in accounts)
            {
                if (account.AccountNumber == accountNumber)
                {
                    return account;
                }
            }
            return null;
        }
    }
}


namespace ATMApplication
{
    class AtmApplication
    {
        private Bank bank;

        public AtmApplication()
        {
            bank = new Bank();
        }

        public void Run()
        {
            while (true)
            {
                Console.WriteLine("Welcome to the ATM application:");
                Console.WriteLine("1. Create Account");
                Console.WriteLine("2. Select Account");
                Console.WriteLine("3. Exit");

                string choice = Console.ReadLine();

                switch (choice)
                {
                    case "1":
                        CreateAccount();
                        break;
                    case "2":
                        SelectAccount();
                        break;
                    case "3":
                        return;
                    default:
                        Console.WriteLine("Invalid choice. Please try again.");
                        break;
                }
            }
        }

        private void CreateAccount()
        {
            Console.Write("Enter Account Holder's Name: ");
            string accountHolderName = Console.ReadLine();
            Console.Write("Enter Account Number: ");
            int accountNumber = int.Parse(Console.ReadLine());
            Console.Write("Enter Interest Rate: ");
            double interestRate = double.Parse(Console.ReadLine());
            Console.Write("Enter Initial Balance: ");
            double initialBalance = double.Parse(Console.ReadLine());

            Account account = new Account(accountNumber, initialBalance, interestRate, accountHolderName);
            bank.AddAccount(account);

            Console.WriteLine("Account created successfully.");
        }

        private void SelectAccount()
        {
            Console.Write("Enter Account Number: ");
            int accountNumber = int.Parse(Console.ReadLine());

            Account account = bank.RetrieveAccount(accountNumber);

            if (account != null)
            {
                while (true)
                {
                    Console.WriteLine("Account Menu:");
                    Console.WriteLine("1. Check Balance");
                    Console.WriteLine("2. Deposit");
                    Console.WriteLine("3. Withdraw");
                    Console.WriteLine("4. Display Transactions");
                    Console.WriteLine("5. Exit Account");

                    string choice = Console.ReadLine();

                    switch (choice)
                    {
                        case "a":
                            Console.WriteLine($"Balance: {account.Balance:C}");
                            break;
                        case "b":
                            Console.Write("Enter deposit amount: ");
                            double depositAmount = double.Parse(Console.ReadLine());
                            account.Deposit(depositAmount);
                            break;
                        case "c":
                            Console.Write("Enter withdrawal amount: ");
                            double withdrawalAmount = double.Parse(Console.ReadLine());
                            account.Withdraw(withdrawalAmount);
                            break;
                        case "d":
                            account.DisplayTransactions();
                            break;
                        case "e":
                            return;
                        default:
                            Console.WriteLine("Invalid choice. Please try again.");
                            break;
                    }
                }
            }
            else
            {
                Console.WriteLine("Account not found.");
            }
        }

        static void Main(string[] args)
        {
            AtmApplication atmApp = new AtmApplication();
            atmApp.Run();
        }
    }
}

