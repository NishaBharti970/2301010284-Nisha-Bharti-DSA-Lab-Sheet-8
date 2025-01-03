# 2301010284-Nisha-Bharti-DSA-Lab-Sheet-8
#include <iostream>
#include <vector>
#include <algorithm>
class Product {
public:
    int id;
    std::string name;
    std::string category;
    float price;
    float rating;
    Product(int id, std::string name, std::string category, float price, float rating) 
        : id(id), name(name), category(category), price(price), rating(rating) {}
};
// Function to display products
void displayProducts(const std::vector<Product>& products) {
    for (const auto& product : products) {
        std::cout << "ID: " << product.id << ", Name: " << product.name 
                  << ", Category: " << product.category << ", Price: " << product.price 
                  << ", Rating: " << product.rating << "\n";
    }
}
// Sorting Algorithms
void insertionSort(std::vector<Product>& products, char criteria) {
    for (int i = 1; i < products.size(); i++) {
        Product key = products[i];
        int j = i - 1;
        while (j >= 0 && ((criteria == 'p' && products[j].price > key.price) || 
                          (criteria == 'r' && products[j].rating > key.rating) || 
                          (criteria == 'n' && products[j].name > key.name))) {
            products[j + 1] = products[j];
            j--;
        }
        products[j + 1] = key;
    }
}
int partition(std::vector<Product>& products, int low, int high, char criteria) {
    Product pivot = products[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if ((criteria == 'p' && products[j].price <= pivot.price) ||
            (criteria == 'r' && products[j].rating <= pivot.rating) ||
            (criteria == 'n' && products[j].name <= pivot.name)) {
            i++;
            std::swap(products[i], products[j]);
        }
    }
    std::swap(products[i + 1], products[high]);
    return i + 1;
}
void quickSort(std::vector<Product>& products, int low, int high, char criteria) {
    if (low < high) {
        int pi = partition(products, low, high, criteria);
        quickSort(products, low, pi - 1, criteria);
        quickSort(products, pi + 1, high, criteria);
    }
}
// Binary Search
int binarySearchById(const std::vector<Product>& products, int id, int low, int high) {
    if (high >= low) {
        int mid = low + (high - low) / 2;
        if (products[mid].id == id)
            return mid;
        if (products[mid].id > id)
            return binarySearchById(products, id, low, mid - 1);
        return binarySearchById(products, id, mid + 1, high);
    }
    return -1;
}
// Sequential Search
int sequentialSearchByName(const std::vector<Product>& products, const std::string& name) {
    for (int i = 0; i < products.size(); i++) {
        if (products[i].name == name) return i;
    }
    return -1;
}
// Main menu to interact with the system
void menu() {
    std::vector<Product> products;
    int choice;
    do {
        std::cout << "\n1. Add Product\n2. Update Product\n3. Delete Product\n4. Display Products\n";
        std::cout << "5. Sort Products\n6. Search Product\n0. Exit\nEnter choice: ";
        std::cin >> choice;
        if (choice == 1) {
            int id;
            std::string name, category;
            float price, rating;
            std::cout << "Enter Product ID: "; std::cin >> id;
            std::cout << "Enter Product Name: "; std::cin >> name;
            std::cout << "Enter Category: "; std::cin >> category;
            std::cout << "Enter Price: "; std::cin >> price;
            std::cout << "Enter Rating: "; std::cin >> rating;
            products.emplace_back(id, name, category, price, rating);
        }
        else if (choice == 2) {
            int id;
            std::cout << "Enter Product ID to update: ";
            std::cin >> id;
            int index = binarySearchById(products, id, 0, products.size() - 1);
            if (index != -1) {
                std::cout << "Enter New Name: "; std::cin >> products[index].name;
                std::cout << "Enter New Category: "; std::cin >> products[index].category;
                std::cout << "Enter New Price: "; std::cin >> products[index].price;
                std::cout << "Enter New Rating: "; std::cin >> products[index].rating;
            } else {
                std::cout << "Product not found.\n";
            }
        }
        else if (choice == 3) {
            int id;
            std::cout << "Enter Product ID to delete: ";
            std::cin >> id;
            int index = binarySearchById(products, id, 0, products.size() - 1);
            if (index != -1) {
                products.erase(products.begin() + index);
                std::cout << "Product deleted.\n";
            } else {
                std::cout << "Product not found.\n";
            }
        }
        else if (choice == 4) {
            displayProducts(products);
        }
        else if (choice == 5) {
            char criteria;
            std::cout << "Choose sort criteria (p: Price, r: Rating, n: Name): ";
            std::cin >> criteria;
            std::cout << "Choose sort type (1: Insertion Sort, 2: Quick Sort): ";
            int sortType;
            std::cin >> sortType;
            if (sortType == 1) {
                insertionSort(products, criteria);
            } else if (sortType == 2) {
                quickSort(products, 0, products.size() - 1, criteria);
            }
            std::cout << "Products sorted.\n";
        }
        else if (choice == 6) {
            int searchChoice;
            std::cout << "Search by 1: ID, 2: Name: ";
            std::cin >> searchChoice;
            if (searchChoice == 1) {
                int id;
                std::cout << "Enter Product ID: ";
                std::cin >> id;
                int index = binarySearchById(products, id, 0, products.size() - 1);
                if (index != -1)
                    std::cout << "Product Found: ID: " << products[index].id << ", Name: " << products[index].name << "\n";
                else
                    std::cout << "Product not found.\n";
            } else if (searchChoice == 2) {
                std::string name;
                std::cout << "Enter Product Name: ";
                std::cin >> name;
                int index = sequentialSearchByName(products, name);
                if (index != -1)
                    std::cout << "Product Found: ID: " << products[index].id << ", Name: " << products[index].name << "\n";
                else
                    std::cout << "Product not found.\n";
            }
        }
    } while (choice != 0);
}
int main() {
    menu();
    return 0;
}
