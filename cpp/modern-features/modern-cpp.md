# Modern C++ Features (C++11 and Later)

Modern C++ (C++11 and later standards) introduced many powerful features that make C++ code more expressive, safer, and easier to write. This guide covers the most important modern C++ features that every developer should know.

## Auto Keyword

The `auto` keyword allows the compiler to automatically deduce the type of a variable from its initializer. This reduces code verbosity and makes it easier to maintain.

### Basic Auto Usage

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main() {
    // Basic type deduction
    auto x = 42;           // x is int
    auto y = 3.14;         // y is double
    auto z = 'a';          // z is char
    auto name = "Hello";   // name is const char*
    
    // With STL containers
    auto numbers = vector<int>{1, 2, 3, 4, 5};
    auto text = string("Modern C++");
    
    // Auto with function return types
    auto result = sqrt(16.0);  // result is double
    
    cout << "x: " << x << " (type: int)" << endl;
    cout << "y: " << y << " (type: double)" << endl;
    cout << "text: " << text << endl;
    
    return 0;
}
```

### Auto with Complex Types

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    // Auto with complex types
    auto grades = map<string, int>{
        {"Alice", 95},
        {"Bob", 87},
        {"Charlie", 92}
    };
    
    // Much cleaner than: map<string, int>::iterator it = grades.begin();
    for (auto it = grades.begin(); it != grades.end(); ++it) {
        cout << it->first << ": " << it->second << endl;
    }
    
    return 0;
}
```

## Lambda Expressions

Lambda expressions allow you to define anonymous functions inline. They are particularly useful for short functions used with STL algorithms.

### Lambda Syntax

```cpp
[capture](parameters) -> return_type { body }
```

### Basic Lambda Examples

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    // Simple lambda with no parameters
    auto greet = []() {
        cout << "Hello from lambda!" << endl;
    };
    greet();
    
    // Lambda with parameters
    auto add = [](int a, int b) {
        return a + b;
    };
    cout << "Sum: " << add(5, 3) << endl;
    
    // Lambda with capture by value
    int multiplier = 10;
    auto multiply = [multiplier](int x) {
        return x * multiplier;
    };
    cout << "Result: " << multiply(5) << endl;
    
    // Lambda with capture by reference
    int counter = 0;
    auto increment = [&counter]() {
        counter++;
        cout << "Counter: " << counter << endl;
    };
    increment();
    increment();
    
    return 0;
}
```

### Lambda with STL Algorithms

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // Find even numbers
    auto even_count = count_if(numbers.begin(), numbers.end(), [](int n) {
        return n % 2 == 0;
    });
    cout << "Even numbers: " << even_count << endl;
    
    // Transform each element
    vector<int> doubled;
    transform(numbers.begin(), numbers.end(), back_inserter(doubled), [](int n) {
        return n * 2;
    });
    
    cout << "Doubled: ";
    for (const auto& num : doubled) {
        cout << num << " ";
    }
    cout << endl;
    
    return 0;
}
```

## Range-Based For Loops

Range-based for loops provide a clean and concise way to iterate over containers and arrays.

### Basic Range-Based For Loops

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <map>
using namespace std;

int main() {
    // With vectors
    vector<int> numbers = {1, 2, 3, 4, 5};
    cout << "Numbers: ";
    for (const auto& num : numbers) {
        cout << num << " ";
    }
    cout << endl;
    
    // With strings
    string text = "Hello";
    cout << "Characters: ";
    for (const auto& ch : text) {
        cout << ch << " ";
    }
    cout << endl;
    
    // With maps
    map<string, int> scores = {
        {"Alice", 95},
        {"Bob", 87},
        {"Charlie", 92}
    };
    
    cout << "Scores:" << endl;
    for (const auto& [name, score] : scores) {  // C++17 structured bindings
        cout << name << ": " << score << endl;
    }
    
    // Modifying elements
    vector<int> data = {1, 2, 3, 4, 5};
    for (auto& value : data) {
        value *= 2;  // Double each value
    }
    
    cout << "Modified data: ";
    for (const auto& value : data) {
        cout << value << " ";
    }
    cout << endl;
    
    return 0;
}
```

### Range-Based For with Arrays

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    
    cout << "Array elements: ";
    for (const auto& element : arr) {
        cout << element << " ";
    }
    cout << endl;
    
    return 0;
}
```

## Smart Pointers Basics

Smart pointers automatically manage memory and help prevent memory leaks and dangling pointers. They are a cornerstone of modern C++ memory management.

### std::unique_ptr

`std::unique_ptr` represents exclusive ownership of a resource. It cannot be copied but can be moved.

```cpp
#include <iostream>
#include <memory>
using namespace std;

class Person {
public:
    string name;
    int age;
    
    Person(const string& n, int a) : name(n), age(a) {
        cout << "Person " << name << " created" << endl;
    }
    
    ~Person() {
        cout << "Person " << name << " destroyed" << endl;
    }
    
    void introduce() {
        cout << "Hi, I'm " << name << ", " << age << " years old" << endl;
    }
};

int main() {
    // Creating unique_ptr
    auto person1 = make_unique<Person>("Alice", 25);
    person1->introduce();
    
    // Moving ownership
    auto person2 = move(person1);
    // person1 is now null
    if (!person1) {
        cout << "person1 is null after move" << endl;
    }
    
    person2->introduce();
    
    // Automatic cleanup when person2 goes out of scope
    return 0;
}
```

### std::shared_ptr

`std::shared_ptr` allows multiple pointers to share ownership of the same resource. The resource is automatically deleted when the last `shared_ptr` is destroyed.

```cpp
#include <iostream>
#include <memory>
#include <vector>
using namespace std;

class Resource {
public:
    string data;
    
    Resource(const string& d) : data(d) {
        cout << "Resource '" << data << "' created" << endl;
    }
    
    ~Resource() {
        cout << "Resource '" << data << "' destroyed" << endl;
    }
};

int main() {
    // Creating shared_ptr
    auto resource1 = make_shared<Resource>("Important Data");
    cout << "Reference count: " << resource1.use_count() << endl;
    
    // Sharing ownership
    auto resource2 = resource1;
    cout << "Reference count after copy: " << resource1.use_count() << endl;
    
    // Using in containers
    vector<shared_ptr<Resource>> resources;
    resources.push_back(resource1);
    resources.push_back(resource2);
    
    cout << "Reference count after adding to vector: " << resource1.use_count() << endl;
    
    // Reset one pointer
    resource2.reset();
    cout << "Reference count after reset: " << resource1.use_count() << endl;
    
    // Resource will be automatically cleaned up when all shared_ptrs are destroyed
    return 0;
}
```

### Smart Pointer Best Practices

```cpp
#include <iostream>
#include <memory>
#include <vector>
using namespace std;

// Factory function returning unique_ptr
unique_ptr<int> createNumber(int value) {
    return make_unique<int>(value);
}

// Function accepting unique_ptr by reference (doesn't transfer ownership)
void processNumber(const unique_ptr<int>& num) {
    cout << "Processing number: " << *num << endl;
}

// Function accepting shared_ptr
void shareNumber(shared_ptr<int> num) {
    cout << "Sharing number: " << *num << endl;
    cout << "Reference count in function: " << num.use_count() << endl;
}

int main() {
    // Use make_unique and make_shared
    auto num1 = make_unique<int>(42);
    auto num2 = make_shared<int>(100);
    
    // Process without transferring ownership
    processNumber(num1);
    
    // Share ownership
    shareNumber(num2);
    cout << "Reference count after function: " << num2.use_count() << endl;
    
    // Use smart pointers in containers
    vector<unique_ptr<int>> numbers;
    numbers.push_back(make_unique<int>(1));
    numbers.push_back(make_unique<int>(2));
    numbers.push_back(make_unique<int>(3));
    
    cout << "Numbers in vector: ";
    for (const auto& num : numbers) {
        cout << *num << " ";
    }
    cout << endl;
    
    return 0;
}
```

## Combining Modern Features

Here's an example that combines multiple modern C++ features:

```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <algorithm>
#include <string>
using namespace std;

class Task {
public:
    string description;
    int priority;
    
    Task(const string& desc, int prio) : description(desc), priority(prio) {}
    
    void execute() const {
        cout << "Executing: " << description << " (Priority: " << priority << ")" << endl;
    }
};

int main() {
    // Create tasks using modern features
    auto tasks = vector<shared_ptr<Task>>{
        make_shared<Task>("Write code", 1),
        make_shared<Task>("Review code", 2),
        make_shared<Task>("Test application", 1),
        make_shared<Task>("Deploy to production", 3)
    };
    
    // Sort by priority using lambda
    sort(tasks.begin(), tasks.end(), [](const auto& a, const auto& b) {
        return a->priority < b->priority;
    });
    
    // Execute high priority tasks (priority 1 and 2) using modern loops and lambdas
    cout << "Executing high priority tasks:" << endl;
    for (const auto& task : tasks) {
        if (task->priority <= 2) {
            task->execute();
        }
    }
    
    // Count tasks by priority using lambda
    auto high_priority_count = count_if(tasks.begin(), tasks.end(), [](const auto& task) {
        return task->priority == 1;
    });
    
    cout << "High priority tasks: " << high_priority_count << endl;
    
    return 0;
}
```

## Summary

Modern C++ features make code:
- **More expressive**: `auto`, lambdas, and range-based for loops
- **Safer**: Smart pointers prevent memory leaks
- **More maintainable**: Type deduction reduces code duplication
- **More efficient**: Move semantics and smart pointers improve performance

These features work together to create cleaner, safer, and more maintainable C++ code. Start incorporating them into your projects to write modern, idiomatic C++!