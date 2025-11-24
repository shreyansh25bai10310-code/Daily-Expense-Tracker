# Daily-Expense-Tracker
import csv
import os
from datetime import datetime

FILE_NAME = "expenses.csv"


def initialize_file():
    """Create the CSV file if it doesn't exist."""
    if not os.path.exists(FILE_NAME):
        with open(FILE_NAME, mode="w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Date", "Category", "Amount", "Description"])


def add_expense():
    """Add an expense to the CSV file."""
    date = datetime.now().strftime("%Y-%m-%d")
    category = input("Enter category (Food, Transport, Bills, etc.): ")
    amount = input("Enter amount: ")
    description = input("Enter description: ")

    with open(FILE_NAME, mode="a", newline="") as file:
        writer = csv.writer(file)
        writer.writerow([date, category, amount, description])

    print("\nExpense added successfully!\n")


def view_expenses():
    """Display all expenses."""
    if not os.path.exists(FILE_NAME):
        print("No expenses recorded yet.\n")
        return

    print("\n--- All Expenses ---")
    with open(FILE_NAME, mode="r") as file:
        reader = csv.reader(file)
        for row in reader:
            print(", ".join(row))
    print()


def total_expenses():
    """Calculate and display the total amount spent."""
    total = 0

    if not os.path.exists(FILE_NAME):
        print("No expenses recorded yet.\n")
        return

    with open(FILE_NAME, mode="r") as file:
        reader = csv.reader(file)
        next(reader)  # Skip header
        for row in reader:
            try:
                total += float(row[2])
            except ValueError:
                pass

    print(f"\nTotal expenses: ${total:.2f}\n")


def main():
    initialize_file()

    while True:
        print("===== Daily Expense Tracker =====")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. View Total Spending")
        print("4. Exit")

        choice = input("Choose an option (1-4): ")

        if choice == "1":
            add_expense()
        elif choice == "2":
            view_expenses()
        elif choice == "3":
            total_expenses()
        elif choice == "4":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try again.\n")


if __name__ == "__main__":
    main()
