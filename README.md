from datetime import datetime, timedelta

class LibraryItem:
    def __init__(self, title, author, category):
        self.title = title
        self.author = author
        self.category = category
        self.is_checked_out = False
        self.due_date = None
    def check_out(self):
        if not self.is_checked_out:
            self.is_checked_out = True
            self.due_date = datetime.now() + timedelta(days=14)
            print(f"{self.title} checked out successfully. Due date: {self.due_date.date()}")
        else:
            print(f"{self.title} is already checked out.")
    def return_item(self):
        if self.is_checked_out:
            self.is_checked_out = False
            overdue_days = (datetime.now() - self.due_date).days
            fine = max(0, overdue_days * 0.5)  # 0.5 per day overdue
            print(f"{self.title} returned. Overdue fine: ${fine:.2f}")
            self.due_date = None
        else:
            print(f"{self.title} is not checked out.")

class Library:
    def __init__(self):
        self.items = []
    def add_item(self, title, author, category):
        item = LibraryItem(title, author, category)
        self.items.append(item)
        print(f"Added new item: {title} by {author} (Category: {category})")
    def search_items(self, search_term, search_type="title"):
        results = []
        for item in self.items:
            if (search_type == "title" and search_term.lower() in item.title.lower()) or \
               (search_type == "author" and search_term.lower() in item.author.lower()) or \
               (search_type == "category" and search_term.lower() == item.category.lower()):
                results.append(item)
        if results:
            print(f"\nSearch Results for '{search_term}' in {search_type}:")
            for result in results:
                status = "Available" if not result.is_checked_out else f"Checked out, due: {result.due_date.date()}"
                print(f"Title: {result.title}, Author: {result.author}, Category: {result.category}, Status: {status}")
        else:
            print(f"No items found for '{search_term}' in {search_type}.")
    def check_out_item(self, title):
        for item in self.items:
            if item.title.lower() == title.lower():
                item.check_out()
                return
        print(f"No item found with the title '{title}'.")
    def return_item(self, title):
        for item in self.items:
            if item.title.lower() == title.lower():
                item.return_item()
                return
        print(f"No item found with the title '{title}'.")

# Example usage
library = Library()
library.add_item("The Great Gatsby", "F. Scott Fitzgerald", "Book")
library.add_item("National Geographic", "Various Authors", "Magazine")
library.add_item("Inception", "Christopher Nolan", "DVD")

library.search_items("Great", "title")
library.check_out_item("The Great Gatsby")
library.search_items("F. Scott Fitzgerald", "author")

library.return_item("The Great Gatsby")
library.search_items("Book", "category")
