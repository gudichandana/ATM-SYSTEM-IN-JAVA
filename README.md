// ATM System :

// Class and Objects
// Constructor methods
// Collections
// Exception Handling


import java.util.*;

class ATM {
    private String accountName;
    private double balance;
    private int pin;
    private LinkedList<String> transactions;

    // Constructor to initialize account
    public ATM(String accountName, double initialBalance, int pin) {
        this.accountName = accountName;
        this.balance = initialBalance;
        this.pin = pin;
        this.transactions = new LinkedList<>();
    }

    // Verify entered PIN
    public boolean verifyPin(int enteredPin) {
        return this.pin == enteredPin;
    }

    // Show account name and balance
    public void displayDetails() {
        System.out.println("\n=== Account Details ===");
        System.out.println("Name: " + accountName);
        System.out.println("Balance: ₹" + balance);
    }

    // Just show balance
    public void displayBalanceOnly() {
        System.out.println("Current Balance: ₹" + balance);
    }

    // Deposit amount
    public void deposit(double amount) {
        try {
            if (amount <= 0) {
                throw new IllegalArgumentException("Amount must be positive.");
            }
            balance += amount;
            addTransaction("Deposited ₹" + amount);
            System.out.println("₹" + amount + " deposited successfully.");
        } catch (Exception e) {
            System.out.println("Deposit failed: " + e.getMessage());
        }
    }

    // Withdraw amount
    public void withdraw(double amount) {
        try {
            if (amount <= 0 || amount > balance) {
                throw new IllegalArgumentException("Invalid or insufficient balance.");
            }
            balance -= amount;
            addTransaction("Withdrew ₹" + amount);
            System.out.println("₹" + amount + " withdrawn successfully.");
        } catch (Exception e) {
            System.out.println("Withdrawal failed: " + e.getMessage());
        }
    }

    // Transfer funds to another (simulated)
    public void transferFunds(String receiver, double amount) {
        try {
            if (amount <= 0 || amount > balance) {
                throw new IllegalArgumentException("Invalid or insufficient balance for transfer.");
            }
            balance -= amount;
            addTransaction("Transferred ₹" + amount + " to " + receiver);
            System.out.println("₹" + amount + " transferred to " + receiver);
        } catch (Exception e) {
            System.out.println("Transfer failed: " + e.getMessage());
        }
    }

    // Change PIN securely
    public void changePin(Scanner sc) {
        try {
            System.out.print("Enter current PIN: ");
            int oldPin = sc.nextInt();
            if (oldPin != pin) {
                throw new SecurityException("Incorrect current PIN.");
            }

            System.out.print("Enter new 4-digit PIN: ");
            int newPin = sc.nextInt();
            if (String.valueOf(newPin).length() != 4) {
                throw new IllegalArgumentException("PIN must be exactly 4 digits.");
            }

            pin = newPin;
            System.out.println("PIN changed successfully.");
        } catch (Exception e) {
            System.out.println("PIN change failed: " + e.getMessage());
        }
    }

    // Add transaction to list (latest first)
    private void addTransaction(String detail) {
        String timestamp = new Date().toString();
        transactions.addFirst(detail + " on " + timestamp);
        if (transactions.size() > 5) {
            transactions.removeLast();
        }
    }

    // Show last 5 transactions
    public void printLastTransactions() {
        System.out.println("\n=== Last 5 Transactions ===");
        if (transactions.isEmpty()) {
            System.out.println("No transactions yet Happend.");
        } else {
            for (String t : transactions) {
                System.out.println(t);
            }
        }
    }
}

public class ATMApp {
    
    public static void main(String[] args) {
        
        Scanner sc = new Scanner(System.in);
        
        ATM atm = new ATM("Prasath", 1000.0, 1234); // Initial account

        // === PIN Verification ===
        System.out.println("Welcome to SVCE ATM :");
        System.out.print("Enter your 4-digit PIN to access ATM: ");
        int enteredPin = sc.nextInt();
        if (!atm.verifyPin(enteredPin)) {
            System.out.println("Incorrect PIN. Access Denied.");
            return;
        }

        int choice;
        do {
            // Display Menu
            System.out.println("\n=== ATM Menu ===");
            System.out.println("1. View Account Details");
            System.out.println("2. Deposit");
            System.out.println("3. Withdraw");
            System.out.println("4. Show Last 5 Transactions");
            System.out.println("5. Check Balance Only");
            System.out.println("6. Change PIN");
            System.out.println("7. Transfer Funds");
            System.out.println("8. Exit");
            System.out.print("Enter choice: ");

            try {
                choice = sc.nextInt();

                switch (choice) {
                    case 1: {
                        atm.displayDetails();
                        break;
                    }
                    case 2: {
                        System.out.print("Enter deposit amount: ₹");
                        double dep = sc.nextDouble();
                        atm.deposit(dep);
                        break;
                    }
                    case 3: {
                        System.out.print("Enter withdrawal amount: ₹");
                        double wd = sc.nextDouble();
                        atm.withdraw(wd);
                        break;
                    }
                    case 4: {
                        atm.printLastTransactions();
                        break;
                    }
                    case 5: {
                        atm.displayBalanceOnly();
                        break;
                    }
                    case 6: {
                        atm.changePin(sc);
                        break;
                    }
                    case 7: {
                        sc.nextLine(); // Clear buffer
                        System.out.print("Enter receiver's name: ");
                        String receiver = sc.nextLine();
                        System.out.print("Enter amount to transfer: ₹");
                        double amt = sc.nextDouble();
                        atm.transferFunds(receiver, amt);
                        break;
                    }
                    case 8: {
                        System.out.println("Thank you for using the ATM.");
                        break;
                    }
                    default: {
                        System.out.println("Invalid choice. Try again.");
                        break;
                    }
                }

            } catch (InputMismatchException e) {
                System.out.println("Please enter valid input!");
                sc.nextLine(); // Clear invalid input
                choice = -1;
            }

        } while (choice != 8); // Continue until user chooses to exit

        sc.close(); // Close scanner resource
    }
}
