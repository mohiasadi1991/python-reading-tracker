

import json
from pathlib import Path

FILE = Path("books.json")

def load_books():
    if FILE.exists():
        return json.loads(FILE.read_text(encoding="utf-8"))
    return []

def save_books(books):
    FILE.write_text(
        json.dumps(books, ensure_ascii=False, indent=4),
        encoding="utf-8"
    )

def add_book(books):
    title = input("Book title: ").strip()
    author = input("Author: ").strip()

    if not title:
        print("Title cannot be empty.")
        return

    books.append({
        "title": title,
        "author": author,
        "completed": False
    })
    save_books(books)
    print("Book added successfully!")

def show_books(books):
    if not books:
        print("No books found.")
        return

    for index, book in enumerate(books, 1):
        status = "Completed" if book["completed"] else "Reading"
        print(f"{index}. {book['title']} - {book['author']} [{status}]")

def complete_book(books):
    show_books(books)

    try:
        number = int(input("Book number to complete: "))
        if 1 <= number <= len(books):
            books[number - 1]["completed"] = True
            save_books(books)
            print("Book marked as completed!")
        else:
            print("Invalid book number.")
    except ValueError:
        print("Please enter a valid number.")

def show_stats(books):
    total = len(books)
    completed = sum(book["completed"] for book in books)
    print(f"Total books: {total}")
    print(f"Completed books: {completed}")
    print(f"Currently reading: {total - completed}")

def main():
    books = load_books()

    while True:
        print("\n=== Reading Tracker ===")
        print("1. Add a book")
        print("2. Show all books")
        print("3. Complete a book")
        print("4. Reading statistics")
        print("5. Exit")

        choice = input("Choose an option: ")

        if choice == "1":
            add_book(books)
        elif choice == "2":
            show_books(books)
        elif choice == "3":
            complete_book(books)
        elif choice == "4":
            show_stats(books)
        elif choice == "5":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
