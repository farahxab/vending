# vending
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Constants for product names, prices, and quantities
#define PRODUCT_A_NAME 'A'
#define PRODUCT_A_PRICE 1.0
#define PRODUCT_A_QUANTITY 10

#define PRODUCT_B_NAME 'B'
#define PRODUCT_B_PRICE 1.5
#define PRODUCT_B_QUANTITY 15

#define PRODUCT_C_NAME 'C'
#define PRODUCT_C_PRICE 2.0
#define PRODUCT_C_QUANTITY 8

// Constants for admin password and item threshold
#define ADMIN_PASSWORD 1234
#define MIN_QUANTITY 5

// Global variables
float total_amount = 0.0;

// Function prototypes
void displayItems();
void purchaseItem();
void adminMode();
void replenishItems();
void changeItemPrices();
void displayTotalSale();
void displayItemAvailability();

int main() {
    srand(time(0));

    int choice;
    while (1) {
        printf("Vending Machine Simulator\n");
        printf("1. Purchase Item\n");
        printf("2. Admin Mode\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                purchaseItem();
                break;
            case 2:
                adminMode();
                break;
            case 3:
                return 0;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }
}

void displayItems() {
    printf("Available Items:\n");
    printf("%c. %s - $%.2f\n", PRODUCT_A_NAME, "Item A", PRODUCT_A_PRICE);
    printf("%c. %s - $%.2f\n", PRODUCT_B_NAME, "Item B", PRODUCT_B_PRICE);
    printf("%c. %s - $%.2f\n", PRODUCT_C_NAME, "Item C", PRODUCT_C_PRICE);
}

void purchaseItem() {
    displayItems();

    char selectedProduct;
    printf("Enter the product (A, B, C) you want to purchase (0 to cancel): ");
    scanf(" %c", &selectedProduct);

    if (selectedProduct == '0')
        return;

    float price;
    int quantity;

    switch (selectedProduct) {
        case PRODUCT_A_NAME:
            price = PRODUCT_A_PRICE;
            quantity = PRODUCT_A_QUANTITY;
            break;
        case PRODUCT_B_NAME:
            price = PRODUCT_B_PRICE;
            quantity = PRODUCT_B_QUANTITY;
            break;
        case PRODUCT_C_NAME:
            price = PRODUCT_C_PRICE;
            quantity = PRODUCT_C_QUANTITY;
            break;
        default:
            printf("Invalid product selection. Please try again.\n");
            return;
    }

    printf("You selected: %c. Price: $%.2f\n", selectedProduct, price);

    float amountPaid = 0.0;
    while (amountPaid < price) {
        float coin;
        printf("Insert coin (1.00, 0.50, 0.25) or enter 0 to cancel: ");
        scanf("%f", &coin);

        if (coin == 0) {
            printf("Purchase cancelled.\n");
            return;
        }

        if (coin != 1.00 && coin != 0.50 && coin != 0.25) {
            printf("Invalid coin. Please try again.\n");
            continue;
        }

        amountPaid += coin;
    }

    total_amount += price;
    quantity--;
    printf("You purchased item %c for $%.2f. Change: $%.2f\n", selectedProduct, price, amountPaid - price);

    if (quantity <= MIN_QUANTITY)
        printf("Alert: Item %c quantity is less than or equal to %d.\n", selectedProduct, MIN_QUANTITY);
}

void adminMode() {
    int adminPassword;
    printf("Enter the admin password: ");
    scanf("%d", &adminPassword);

    if (adminPassword != ADMIN_PASSWORD) {
   