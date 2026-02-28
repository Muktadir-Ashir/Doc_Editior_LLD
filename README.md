# Building Better Document Editors – A Mini LLD Project

This mini project demonstrates the evolution from a **poorly designed** document editor to a **clean, extensible, and maintainable** one using Object-Oriented Programming (OOP) and SOLID principles.

Two versions are included:

- **Bad Design** → shows common beginner mistakes  
- **Good Design** → applies proper OOP & SOLID principles

## Project Structure
Building docs/
├── BadDesign/
│   └── DocEditor.cpp
├── GoodDesign/
│   └── DocEditor.cpp
├── output/                # generated files (ignored by git)
├── .gitignore
└── README.md


## 1. Bad Design (DocEditor.cpp)

**What it does**  
A simple `DocumentEditor` class that stores both text and images as plain strings in a single `vector<string>`. It guesses the type during rendering by checking file extensions.

**Why this is considered "Bad Design"**

- Violates **Single Responsibility Principle** — one class does everything: storing, rendering, type-checking, saving
- Uses **stringly-typed** data (everything is `string`) → error-prone
- Runtime type checking with string operations (`substr` for ".jpg"/".png") → fragile and hard to extend
- No way to add new element types (bold text, tables, etc.) without modifying existing code
- Mixing of concerns: rendering logic, persistence, and data storage all in one class
- No separation between domain logic and I/O

**Result**: Hard to maintain, test, or extend.

## 2. Good Design (DocEditor.cpp)

**What it does**  
Uses proper OOP to model different document elements (text, image, newline, tab) as separate classes. Rendering is delegated to each element. Saving is separated via a strategy pattern.

**Key Improvements & Design Principles Applied**

### OOP Concepts Used
- **Inheritance** & **Polymorphism**  
  All elements inherit from `DocumentElement` abstract base class and override `render()`.
- **Encapsulation**  
  Each element class owns its data and behavior.
- **Abstraction**  
  Client (DocumentEditor) works with `DocumentElement*` pointers — doesn't care about concrete types.

### SOLID Principles Followed
- **S — Single Responsibility Principle**  
  Each class has one job: `TextElement` renders text, `ImageElement` renders image tag, `FileStorage` only saves to file, etc.
- **O — Open/Closed Principle**  
  Open for extension (add new element types like `BoldTextElement` without changing existing code), closed for modification.
- **L — Liskov Substitution Principle**  
  Any `DocumentElement*` can be used interchangeably.
- **I — Interface Segregation Principle**  
  Small, focused interfaces (`render()` only where needed).
- **D — Dependency Inversion Principle**  
  `DocumentEditor` depends on abstractions (`Document`, `Persistence`) — not concrete classes. Easy to swap `FileStorage` with `DBStorage`.

**Result**: Clean, extensible, testable, and maintainable code.


