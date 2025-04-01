#include <iostream>
#include <string>
using namespace std;

// Structure for passenger details (linked list node)
struct Passenger {              create a hyperlink for this project 
    string name;
    int age;
    string flightNumber;
    Passenger* next;
};

// Flight class (array-based)
class Flight {
public:
    string flightNumber;
    string destination;
    int availableSeats;

    Flight(string fn, string dest, int seats) {
        flightNumber = fn;
        destination = dest;
        availableSeats = seats;
    }
};

// Airline Reservation System
class AirlineSystem {
private:
    Passenger* head; // Linked list head
    Flight flights[3] = {  // Array of flights
        Flight("AI101", "Delhi", 10),
        Flight("AI202", "Mumbai", 8),
        Flight("AI303", "Bangalore", 6)
    };

public:
    AirlineSystem() { head = nullptr; }

    // Book a ticket (User selects flight with 1,2,3)
    void bookTicket() {
        string name, flightNumber;
        int age, flightChoice;

        cout << "Enter Your Name: ";
        cin.ignore();  
        getline(cin, name);  // Allow names with spaces

        cout << "Enter Your Age: ";
        cin >> age;
        cin.ignore();

        // Show available flights as options
        cout << "Select a Flight:\n";
        cout << "1. AI101 (Delhi) - Seats Available: " << flights[0].availableSeats << "\n";
        cout << "2. AI202 (Mumbai) - Seats Available: " << flights[1].availableSeats << "\n";
        cout << "3. AI303 (Bangalore) - Seats Available: " << flights[2].availableSeats << "\n";
        cout << "Enter your choice (1, 2, 3): ";
        cin >> flightChoice;

        // Validate user choice
        if (flightChoice < 1 || flightChoice > 3) {
            cout << "Invalid choice! Booking failed.\n";
            return;
        }

        flightNumber = flights[flightChoice - 1].flightNumber; // Map choice to flight number

        if (flights[flightChoice - 1].availableSeats > 0) {
            Passenger* newPassenger = new Passenger{name, age, flightNumber, head};
            head = newPassenger;
            flights[flightChoice - 1].availableSeats--;
            cout << "Booking confirmed for " << name << " on flight " << flightNumber << "!\n";
        } else {
            cout << "Sorry, this flight is fully booked!\n";
        }
    }

    // Cancel a ticket
    void cancelTicket() {
        string name, flightNumber;
        int flightChoice;

        cout << "Enter Your Name: ";
        cin.ignore();
        getline(cin, name);

        // Show flights for cancellation
        cout << "Select the Flight to Cancel:\n";
        cout << "1. AI101 (Delhi)\n";
        cout << "2. AI202 (Mumbai)\n";
        cout << "3. AI303 (Bangalore)\n";
        cout << "Enter your choice (1, 2, 3): ";
        cin >> flightChoice;

        if (flightChoice < 1 || flightChoice > 3) {
            cout << "Invalid choice! Cancellation failed.\n";
            return;
        }

        flightNumber = flights[flightChoice - 1].flightNumber; // Map choice to flight number

        Passenger* temp = head, *prev = nullptr;
        while (temp != nullptr) {
            if (temp->name == name && temp->flightNumber == flightNumber) {
                if (prev == nullptr) {
                    head = temp->next;
                } else {
                    prev->next = temp->next;
                }
                delete temp;
                flights[flightChoice - 1].availableSeats++; // Restore seat
                cout << "Ticket cancelled for " << name << " on flight " << flightNumber << "!\n";
                return;
            }
            prev = temp;
            temp = temp->next;
        }
        cout << "No booking found for " << name << " on flight " << flightNumber << "!\n";
    }

    // Check flight availability
    void checkAvailability() {
        cout << "\nFlight Availability:\n";
        for (int i = 0; i < 3; i++) {
            cout << flights[i].flightNumber << " to " << flights[i].destination
                 << " - Seats Available: " << flights[i].availableSeats << "\n";
        }
    }
};

int main() {
    AirlineSystem airline;
    int choice;

    do {
        cout << "\nIndian Airlines Reservation System\n";
        cout << "1. Book Ticket\n2. Cancel Ticket\n3. Check Flight Availability\n4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();  

        switch (choice) {
            case 1:
                airline.bookTicket();
                break;
            case 2:
                airline.cancelTicket();
                break;
            case 3:
                airline.checkAvailability();
                break;
            case 4:
                cout << "Exiting system. Thank you!\n";
                break;
            default:
                cout << "Invalid choice! Try again.\n";
        }
    } while (choice != 4);

    return 0;
}

