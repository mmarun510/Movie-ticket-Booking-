package com.TicketBooking.com;

import java.util.Scanner;

public class MovieTicketBookingSystem 
{
    // 5x5 Seating Matrix: 
    private static final int ROWS = 5;
    private static final int COLS = 5;
    private static boolean[][] seats = new boolean[ROWS][COLS];
    
    // Pricing Config
    private static final double BASE_PRICE = 150.0;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        boolean running = true;

        System.out.println("=== Welcome to the Movie Ticket Booking System ===");

        while (running) {
            System.out.println("\n--- MAIN MENU ---");
            System.out.println("1. Show Available Seats");
            System.out.println("2. Book Ticket");
            System.out.println("3. Cancel Ticket");
            System.out.println("4. Movie Rating & Review");
            System.out.println("5. Exit");
            System.out.print("Enter your choice: ");
            
            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    showAvailableSeats();
                    break;
                case 2:
                    System.out.print("Enter Row (0-4): ");
                    int bookRow = scanner.nextInt();
                    System.out.print("Enter Column (0-4): ");
                    int bookCol = scanner.nextInt();
                    System.out.print("Is it a weekend? (true/false): ");
                    boolean isWeekend = scanner.nextBoolean();
                    System.out.print("Enter Coupon Code (or 'none'): ");
                    String coupon = scanner.next();
                    
                    bookTicket(bookRow, bookCol, isWeekend, coupon);
                    break;
                case 3:
                    System.out.print("Enter Row (0-4) to cancel: ");
                    int cancelRow = scanner.nextInt();
                    System.out.print("Enter Column (0-4) to cancel: ");
                    int cancelCol = scanner.nextInt();
                    
                    cancelTicket(cancelRow, cancelCol);
                    break;
                case 4:
                    System.out.print("Enter Movie Rating score (1 to 5): ");
                    int score = scanner.nextInt();
                    displayMovieRatingLogic(score);
                    break;
                case 5:
                    running = false;
                    System.out.println("Thank you for using the booking system!");
                    break;
                default:
                    System.out.println("Invalid choice! Please select between 1 and 5.");
            }
        }
        scanner.close();
    }

   
   //  Show Available Seats
    
    public static void showAvailableSeats() {
        System.out.println("\n--- Seating Chart ([A] = Available, [B] = Booked) ---");
        System.out.print("    ");
        for (int c = 0; c < COLS; c++) {
            System.out.print("Col " + c +  " ");
        }
        System.out.println();

        for (int r = 0; r < ROWS; r++) {
            System.out.print("Row " + r + "   ");
            for (int c = 0; c < COLS; c++) {
                if (!seats[r][c]) {
                    System.out.print("[A]   "); // Available
                } else {
                    System.out.print("[B]   "); // Booked
                }
            }
            System.out.println();
        }
    }

    
    //  Book Ticket, Weekend Pricing & Discount Coupons
     
    public static void bookTicket(int row, int col, boolean isWeekend, String couponCode) {
        // Condition handling for out-of-bound indices
        if (row < 0 || row >= ROWS || col < 0 || col >= COLS) {
            System.out.println("Error: Invalid row or column seat entry!");
            return;
        }

        // Condition handling for availability
        if (seats[row][col]) {
            System.out.println("Sorry! This seat is already booked.");
            return;
        }

        // Apply Weekend Pricing rule
        double finalPrice = BASE_PRICE;
        if (isWeekend) {
            finalPrice += 50.0; 
            System.out.println("Weekend Pricing applied: +$50.00 surcharge.");
        }

        // Apply Discount Coupon rules
        if (couponCode.equalsIgnoreCase("SAVE10")) {
            finalPrice -= finalPrice * 0.10; 
            System.out.println("Coupon 'SAVE10' applied! 10% discount subtracted.");
        } else if (couponCode.equalsIgnoreCase("FLAT20")) {
            finalPrice -= 20.0; 
            System.out.println("Coupon 'FLAT20' applied! $20.00 subtracted.");
        }

        
        seats[row][col] = true; //show B
        System.out.printf("Success! Seat booked at Row %d, Col %d. Total Price: $%.2f%n", row, col, finalPrice);
    }

    // Cancel Ticket
 
    public static void cancelTicket(int row, int col) {
        if (row < 0 || row >= ROWS || col < 0 || col >= COLS) {
            System.out.println("Error: Invalid row or column selection.");
            return;
        }

        if (!seats[row][col]) {
            System.out.println("This seat is already vacant. Nothing to cancel.");
            return;
        }

        seats[row][col] = false; //change B to A
        System.out.printf("Cancellation successful. Seat at Row %d, Col %d is now vacant.%n", row, col);
    }

   
      // Movie Rating Logic
    
    public static void displayMovieRatingLogic(int score) {
        System.out.print("Movie Review is : ");
        if (score == 5) {
            System.out.println(" * * * * * [Masterpiece]");
        } else if (score == 4) {
            System.out.println(" * * * * [Great Watch]");
       
        } else if (score == 3) {
            System.out.println("* * *   [Average / Mixed]");
        } else if (score == 2 || score == 1) {
            System.out.println(" * Poor / Avoid]");
        } else {
            System.out.println("Invalid numeric range! Please input a value between 1 and 5.");
        }
    }
}
