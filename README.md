#include <iostream>
#include <fstream>
#include <string>
#include <vector>
using namespace std;

struct Password {
    string site;
    string username;
    string password;
};

string currentUser = "";

bool registerUser() {
    string user, pass;
    cout << "Enter new username: ";
    cin >> user;
    cout << "Enter new password: ";
    cin >> pass;

    ofstream file("users.txt", ios::app);
    file << user << " " << pass << endl;
    file.close();

    cout << "Registration successful!\n";
    return true;
}

bool loginUser() {
    string user, pass, u, p;
    cout << "Enter username: ";
    cin >> user;
    cout << "Enter password: ";
    cin >> pass;

    ifstream file("users.txt");
    while (file >> u >> p) {
        if (u == user && p == pass) {
            currentUser = user;
            cout << "Login successful!\n";
            file.close();
            return true;
        }
    }
    file.close();
    cout << "Invalid credentials!\n";
    return false;
}

void addPassword() {
    Password pw;
    cout << "Enter Website: ";
    cin >> pw.site;
    cout << "Enter Username: ";
    cin >> pw.username;
    cout << "Enter Password: ";
    cin >> pw.password;

    ofstream file(currentUser + "_vault.txt", ios::app);
    file << pw.site << " " << pw.username << " " << pw.password << endl;
    file.close();

    cout << "Password saved successfully!\n";
}

void viewPasswords() {
    ifstream file(currentUser + "_vault.txt");
    Password pw;

    cout << "\nStored Passwords:\n";
    cout << "-------------------------\n";
    while (file >> pw.site >> pw.username >> pw.password) {
        cout << "Site: " << pw.site 
             << " | Username: " << pw.username 
             << " | Password: " << pw.password << endl;
    }
    file.close();
}

void passwordManagerMenu() {
    int choice;
    do {
        cout << "\n--- Password Manager ---\n";
        cout << "1. Add Password\n";
        cout << "2. View Passwords\n";
        cout << "3. Logout\n";
        cout << "Choice: ";
        cin >> choice;

        switch (choice) {
            case 1: addPassword(); break;
            case 2: viewPasswords(); break;
            case 3: cout << "Logged out.\n"; break;
            default: cout << "Invalid choice!\n";
        }
    } while (choice != 3);
}

int main() {
    int choice;
    do {
        cout << "\n=== Password Manager System ===\n";
        cout << "1. Register\n";
        cout << "2. Login\n";
        cout << "3. Exit\n";
        cout << "Choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                registerUser();
                break;
            case 2:
                if (loginUser())
                    passwordManagerMenu();
                break;
            case 3:
                cout << "Exiting...\n";
                break;
            default:
                cout << "Invalid choice!\n";
        }
    } while (choice != 3);

    return 0;
}

