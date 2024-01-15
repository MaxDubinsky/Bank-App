#include <algorithm>
#include <iostream>
#include <math.h>
#include <vector>

using namespace std;

class Transaction {
private:
    string type;
    double summa;

public:
    Transaction() {
        this->summa = 0.0;
        this->type = "N/A";
    }

    string getType() { return this->type; }
    double getSumma() { return this->summa; }

    void setType(string type) { this->type = type; }
    void setSumma(double summa) { this->summa = summa; }
};

class Transactions {
private:
    vector<Transaction> transactions;

public:
    void addTransaction(Transaction tr) { this->transactions.push_back(tr); }
    vector<Transaction> getTransactions() { return this->transactions; }
};

class BankAccount {
private:
    char emblem;
    double balance;
    string type;
public:
    BankAccount() { this->emblem = '₴'; this->balance = 0.0; this->type = "Hryvna"; }

    string getType() { return this->type; }
    double getBalance() { return this->balance; }

    // ДОБАВИТИ КОШТИ
    void plusMoney(Transactions& trs) {
        Transaction tr;
        double summ;
        cout << "Enter the amount of money you want to add: ";
        try {
            cin >> summ;
            tr.setType("Plus");
            tr.setSumma(summ);
            trs.addTransaction(tr);
        }
        catch (const exception& e) {
            cout << "Error! " << e.what() << endl;
        }
        this->balance += summ;
    }

    // СНЯТИ КОШТИ
    void minusMoney(Transactions& trs) {
        Transaction tr;
        double summ;
        cout << "Enter the amount of money you want to minus: ";
        cin >> summ;
        if (summ > this->balance) {
            cout << "Error! You don't have enough money! Enter it again: " << endl;
        }
        else {
            this->balance -= summ;
            tr.setType("Minus");
            tr.setSumma(summ);
            trs.addTransaction(tr);
        }
    }

    // ПЕРЕВІРКА БАЛАНСУ
    void checkMoney() { cout << "Your money: " << this->balance << this->emblem << endl; }

    void checkTransactions(Transactions& trs) {
        if (trs.getTransactions().size() > 0) {
            for (int i = 0; i < trs.getTransactions().size(); i++) {
                cout << trs.getTransactions()[i].getType() << ", " << trs.getTransactions()[i].getSumma() << this->emblem << endl;
            }
        }
        else {
            cout << "No transactions. :(\n\n";
        }
    }

    void changeMoney() {
        int choice;
        cout << "Please, choose currency that you want to change.\n";
        cout << "|--------------------------------|\n";
        cout << "|  1 - U.S Dollar                |\n";
        cout << "|  2 - Euro                      |\n";
        cout << "|  3 - Ukrainian Hryvna          |\n";
        cout << "|--------------------------------|\n";
        cout << "Your choice: ";
        cin >> choice;
        switch (choice)
        {
        case 1:
            if (this->type == "Hryvna") {
                this->type = "Dollar";
                this->balance = round(this->balance / 37.83 * 100) / 100.0;
                this->emblem = '$';
            }
            else if (this->type == "Euro") {
                this->type = "Dollar";
                this->balance = round(this->balance / 0.91 * 100) / 100.0;
                this->emblem = '$';
            }
            break;
        case 2:
            if (this->type == "Hryvna") {
                this->type = "Euro";
                this->balance = round(this->balance / 0.02 * 100) / 100.0;
                this->emblem = '€';
            }
            else if (this->type == "Dollar") {
                this->type = "Euro";
                this->balance = round(this->balance / 1.10 * 100) / 100.0;
                this->emblem = '€';
            }
            break;
        case 3:
            if (this->type == "Dollar") {
                this->type = "Hryvna";
                this->balance = round(this->balance / 0.03 * 100) / 100.0;
                this->emblem = '€';
            }
            else if (this->type == "Euro") {
                this->type = "Hryvna";
                this->balance = round(this->balance / 0.02 * 100) / 100.0;
                this->emblem = '€';
            }
            break;
        default:
            cout << "You entered wrong input. Try again.";
        }
    }
};

int main() {
    BankAccount BA;
    Transactions trs;
    int choice = 0;
    cout << "|-------------------------|\n";
    cout << "|  1 - Add money          |\n";
    cout << "|  2 - Withdraw money     |\n";
    cout << "|  3 - Check balance      |\n";
    cout << "|  4 - Check transactions |\n";
    cout << "|  5 - Change Currency    |\n";
    cout << "|-------------------------|\n";
    do {
        printf("Choose one of the options: ");
        cin >> choice;
        switch (choice) {
        case 1:
            BA.plusMoney(trs);
            continue;
        case 2:
            BA.minusMoney(trs);
            continue;
        case 3:
            BA.checkMoney();
            continue;
        case 4:
            BA.checkTransactions(trs);
            continue;
        case 5:
            BA.changeMoney();
            continue;
        default:
            perror("Choose one of variants that wrote on the table. Try again.");
            continue;
        }
    } while (true);
}
