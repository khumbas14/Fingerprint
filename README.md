import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

// User class representing a user in the system
class User {
    private String userId;
    private String fingerprint;

    public User(String userId, String fingerprint) {
        this.userId = userId;
        this.fingerprint = fingerprint;
    }

    public String getUserId() {
        return userId;
    }

    public String getFingerprint() {
        return fingerprint;
    }
}

// FingerprintAuth class to handle registration and authentication
class FingerprintAuth {
    private Map<String, User> userDatabase;

    public FingerprintAuth() {
        userDatabase = new HashMap<>();
    }

    public boolean registerUser(String userId, String fingerprint) {
        if (userDatabase.containsKey(userId)) {
            System.out.println("User ID already exists.");
            return false;
        }
        userDatabase.put(userId, new User(userId, fingerprint));
        System.out.println("User registered successfully.");
        return true;
    }

    public boolean authenticateUser(String userId, String fingerprint) {
        User user = userDatabase.get(userId);
        if (user != null && user.getFingerprint().equals(fingerprint)) {
            System.out.println("Authentication successful.");
            return true;
        }
        System.out.println("Authentication failed.");
        return false;
    }
}

// ATM class simulating the ATM interface
class ATM {
    private FingerprintAuth fingerprintAuth;

    public ATM() {
        fingerprintAuth = new FingerprintAuth();
    }

    public void start() {
        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("ATM Menu:");
            System.out.println("1. Register User");
            System.out.println("2. Authenticate User");
            System.out.println("3. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter User ID: ");
                    String userId = scanner.nextLine();
                    System.out.print("Enter Fingerprint (as string): ");
                    String fingerprint = scanner.nextLine();
                    fingerprintAuth.registerUser(userId, fingerprint);
                    break;
                case 2:
                    System.out.print("Enter User ID: ");
                    userId = scanner.nextLine();
                    System.out.print("Enter Fingerprint (as string): ");
                    fingerprint = scanner.nextLine();
                    fingerprintAuth.authenticateUser(userId, fingerprint);
                    break;
                case 3:
                    System.out.println("Exiting...");
                    scanner.close();
                    return;
                default:
                    System.out.println("Invalid option. Please try again.");
            }
        }
    }
}

// Main class to run the application
public class Main {
    public static void main(String[] args) {
        ATM atm = new ATM();
        atm.start();
    }
}
