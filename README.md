#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// Candy names and prices
const char candy_name_A[] = "Snickers";
const char candy_name_B[] = "M&Ms";
const char candy_name_C[] = "KitKat";

const float candy_price_A = 1.25f;
const float candy_price_B = 1.0f;
const float candy_price_C = 1.5f;

// Vending machine variables
int quantity_A = 10;
int quantity_B = 10;
int quantity_C = 10;

int admin_password = 1234;  // Sample admin password
const int MIN = 5;
float total_amount = 0.0f;

// Function to display the menu for purchasing items
void displayPurchaseMenu() {
    printf("\nItems available for purchase:\n");
    printf("1. %s ($%.2f)\n", candy_name_A, candy_price_A);
    printf("2. %s ($%.2f)\n", candy_name_B, candy_price_B);
    printf("3. %s ($%.2f)\n", candy_name_C, candy_price_C);
    printf("0. Cancel\n");
}

// Function to handle purchasing an item
void purchaseItem() {
    displayPurchaseMenu();

    int choice;
    printf("Enter your choice: ");
    scanf("%d", &choice);

    float selected_price = 0.0f;
    char selected_name[20];

    switch (choice) {
        case 1:
            selected_price = candy_price_A;
            strcpy(selected_name, candy_name_A);
            break;
        case 2:
            selected_price = candy_price_B;
            strcpy(selected_name, candy_name_B);
            break;
        case 3:
            selected_price = candy_price_C;
            strcpy(selected_name, candy_name_C);
            break;
        case 0:
            printf("Purchase cancelled.\n");
            return;
        default:
            printf("Invalid choice. Purchase cancelled.\n");
            return;
    }

    printf("You selected: %s ($%.2f)\n", selected_name, selected_price);
    printf("Please insert coins (1, 0.5, or 0.25) until the price is covered.\n");

    float amount_inserted = 0.0f;
    float coin;
    while (amount_inserted < selected_price) {
        printf("Insert coin: ");
        scanf("%f", &coin);

        if (coin == 1.0f || coin == 0.5f || coin == 0.25f)
            amount_inserted += coin;
        else
            printf("Invalid coin. Try again.\n");
    }

    // Update quantities and total amount
    if (amount_inserted >= selected_price) {
        total_amount += selected_price;
        printf("You have purchased %s for $%.2f. Thank you!\n", selected_name, selected_price);
        printf("Change to return: $%.2f\n", amount_inserted - selected_price);

        if (choice == 1)
            quantity_A--;
        else if (choice == 2)
            quantity_B--;
        else if (choice == 3)
            quantity_C--;

        if (quantity_A <= MIN || quantity_B <= MIN || quantity_C <= MIN)
            printf("ALERT: Quantity of an item is less than or equal to %d!\n", MIN);
    }
}

// Function to handle admin mode
void adminMode() {
    int entered_password;
    printf("Enter admin password: ");
    scanf("%d", &entered_password);

    if (entered_password != admin_password) {
        printf("Incorrect password. Exiting admin mode.\n");
        return;
    }

    while (1) {
        printf("\nAdmin Menu:\n");
        printf("1. Replenish Items\n");
        printf("2. Change Item Prices\n");
        printf("3. Display Total Sale\n");
        printf("4. Display Item Availability\n");
        printf("0. Exit Admin Mode\n");

        int admin_choice;
        printf("Enter your choice: ");
        scanf("%d", &admin_choice);

        switch (admin_choice) {
            case 1:
                // Replenish items
                quantity_A = rand() % 16 + 5;
                quantity_B = rand() % 16 + 5;
                quantity_C = rand() % 16 + 5;
                printf("Items replenished.\n");
                break;
            case 2:
                // Change item prices
                printf("Enter new price for %s: $", candy_name_A);
                scanf("%f", &candy_price_A);
                printf("Enter new price for %s: $", candy_name_B);
                scanf("%f", &candy_price_B);
                printf("Enter new price for %s: $", candy_name_C);
                scanf("%f", &candy_price_C);
                printf("Item prices updated.\n");
                break;
            case 3:
                // Display total sale
                printf("Total Sale Amount: $%.2f\n", total_amount);
                printf("Would you like to reset the total sale amount to zero? (1 for Yes, 0 for No): ");
                int reset_choice;
                scanf("%d", &reset_choice);
                if (reset_choice == 1) {
                    total_amount = 0.0f;
                    printf("Total sale amount reset to zero. Don't forget to collect the money.\n");
                }
                break;
            case 4:
                // Display item availability
                printf("Item Availability:\n");
                printf("%s: %d\n", candy_name_A, quantity_A);
                printf("%s: %d\n", candy_name_B, quantity_B);
                printf("%s: %d\n", candy_name_C, quantity_C);
                break;
            case 0:
                // Exit admin mode
                return;
            default:
                printf("Invalid choice. Try again.\n");
                break;
        }
    }
}

int main() {
    srand(time(NULL));  // Initialize random seed

    while (1) {
        printf("\nMenu:\n");
        printf("1. Purchase Item\n");
        printf("2. Admin Mode\n");
        printf("3. Exit\n");

        int user_choice;
        printf("Enter your choice: ");
        scanf("%d", &user_choice);

        switch (user_choice) {
            case 1:
                purchaseItem();
                break;
            case 2:
                adminMode();
                break;
            case 3:
                printf("Exiting. Thank you!\n");
                exit(0);
            default:
                printf("Invalid choice. Try again.\n");
                break;
        }
    }

    return 0;
}
