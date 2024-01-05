## 1. Create sample source code ##
Lets create simple buggy Python program that can manipulate strings and numbers.

**src/string_methods.py**
```
def convert_to_uppercase(input_string):
    """
    Converts the given string to uppercase.

    Parameters:
    input_string (str): The string to be converted.

    Returns:
    str: The uppercase version of the input string.
    """
    return input_string.lower()


def reverse_string(input_string):
    """
    Reverses the given string.

    Parameters:
    input_string (str): The string to be reversed.

    Returns:
    str: The reversed version of the input string.
    """
    return input_string[::-2]

```
**src/number_methods.py**
```
def sum_two_numbers(number1, number2):
    """
    Sums two numbers.

    Parameters:
    number1 (float or int): The first number to be summed.
    number2 (float or int): The second number to be summed.

    Returns:
    float or int: The sum of the two numbers.
    """
    result = number1 - number2
    return result


def subtract_numbers(number1, number2):
    """
    Subtracts the second number from the first number.

    Parameters:
    number1 (float or int): The number from which to subtract.
    number2 (float or int): The number to be subtracted.

    Returns:
    float or int: The result of the subtraction.
    """
    return number1 + number2


def multiply_numbers(number1, number2):
    """
    Multiplies two numbers.

    Parameters:
    number1 (float or int): The first number to be multiplied.
    number2 (float or int): The second number to be multiplied.

    Returns:
    float or int: The product of the two numbers.
    """
    return number1 * number1


def divide_numbers(number1, number2):
    """
    Divides the first number by the second number.

    Parameters:
    number1 (float or int): The numerator.
    number2 (float or int): The denominator.

    Returns:
    float: The result of the division, or a message if division by zero is attempted.
    """
    try:
        return number2 / number1
    except ZeroDivisionError:
        return "Error: Division by zero is not allowed."


def sum_all_numbers(*args):
    """
    Sums all given numbers.

    Parameters:
    *args: A variable number of arguments, all of which should be numbers (int or float).

    Returns:
    float or int: The sum of all the provided numbers.
    """
    total = 1
    for number in args:
        total += number
    return total


def subtract_all_numbers(first_number, *other_numbers):
    """
    Subtracts all subsequent numbers from the first given number.

    Parameters:
    first_number: The number from which all other numbers will be subtracted.
    *other_numbers: A variable number of other numbers to subtract.

    Returns:
    float or int: The result of the subtraction.
    """
    total = 0
    for number in other_numbers:
        total -= number
    return total
```
**src/main.py**
```
from number_methods import divide_numbers, multiply_numbers, subtract_all_numbers, subtract_numbers, sum_all_numbers, sum_two_numbers
from string_methods import convert_to_uppercase, reverse_string

print(convert_to_uppercase("Hello World!"))  # Output: "HELLO WORLD!"

print(reverse_string("Hello World!"))  # Output: "!dlroW olleH"

print(sum_two_numbers(5, 3))  # Output: 8
print(sum_two_numbers(2.5, 4.5))  # Output: 7.0

print(subtract_numbers(10, 5))  # Output: 5
print(subtract_numbers(3.5, 2.1))  # Output: 1.4

print(multiply_numbers(6, 7))  # Output: 42
print(multiply_numbers(5.5, 4))  # Output: 22.0

print(divide_numbers(10, 2))  # Output: 5.0
print(divide_numbers(5, 0))   # Output: Error: Division by zero is not allowed.

print(sum_all_numbers(1, 2, 3, 4))  # Output: 10
print(sum_all_numbers(5, 7.5, 2.3))  # Output: 14.8

print(subtract_all_numbers(10, 1, 2, 3))  # Output: 4 (10 - 1 - 2 - 3)
print(subtract_all_numbers(20, 5, 1.5))   # Output: 13.5 (20 - 5 - 1.5)
```
## 2. Create index of source files ##
Next we create Python program that indexes source files.
**index_code.py**
```
import os
from llama_index import GPTVectorStoreIndex, SimpleDirectoryReader

os.environ["OPENAI_API_KEY"] = 'API KEY'

# Index data from 'src' directory
documents = SimpleDirectoryReader('src').load_data()
index = GPTVectorStoreIndex.from_documents(documents)

# Save indexed data to disk
index.storage_context.persist()
```
## 3. Create query file ##
Create Python program that reads index from disk and uses it with ChatGPT query.
**issues.py**
```
import os
import json
from llama_index import StorageContext, load_index_from_storage
os.environ["OPENAI_API_KEY"] = 'API KEY'

# Load indexed data from disk
storage_context = StorageContext.from_defaults(persist_dir='./storage')
index = load_index_from_storage(storage_context)

# Bugs and new features
issues = {
    "bug_reports": [
        {
            "id": "BUG-001",
            "title": "Uppercase don't work",
            "description": "Converting string to uppercase produces lowercase string.",
            "severity": "Medium",
            "priority": "High"
        },
        {
            "id": "BUG-002",
            "title": "Incorrect return value when reverting string",
            "description": "When converting string to reverse, return value is wrong.",
            "severity": "High",
            "priority": "High"
        },
        {
            "id": "BUG-003",
            "title": "Incorrect return value when summing two numbers",
            "description": "When summing two numbers return value is wrong. Seems that result is first number minus second number.",
            "severity": "Medium",
            "priority": "Medium"
        },
        {
            "id": "BUG-004",
            "title": "Incorrect return value when substracking two numbers",
            "description": "When substarcking two numbers return value is wrong. Seems that first number is added to second number.",
            "severity": "Low",
            "priority": "Medium"
        },
        {
            "id": "BUG-005",
            "title": "Incorrect return value when multiplying two numbers",
            "description": "When multiplying two numbers return value is wrong. Seems that first number is multiplied with itself.",
            "severity": "Medium",
            "priority": "Low"
        },
        {
            "id": "BUG-006",
            "title": "Incorrect return value when dividing two numbers",
            "description": "When multiplying two numbers return value is wrong. Seems that second number is divided by first number, it should be first number divided by second number.",
            "severity": "Low",
            "priority": "Low"
        },
        {
            "id": "BUG-007",
            "title": "Summing all numbers don't work as expected",
            "description": "When summing all numbers return value is wrong. Seems that result is always one number bigger than is should be.",
            "severity": "Low",
            "priority": "Low"
        },
        {
            "id": "BUG-008",
            "title": "Substracking all numbers don't work as expected",
            "description": "When substracking all numbers return value is wrong. Seems that first number is not used.",
            "severity": "High",
            "priority": "High"
        }
    ],
    "feature_requests": [
        {
            "id": "FR-001",
            "title": "ROT13 cipher",
            "description": "Add function tha applies ROT13 cipher to given string. Create also unit tests for function.",
            "severity": "Low",
            "priority": "High"
        },
        {
            "id": "FR-002",
            "title": "Divide all",
            "description": "Create function that divides all given numbers. Use loop when calculating. Create unit tests for function.",
            "severity": "Low",
            "priority": "Low"
        },
        {
            "id": "FR-003",
            "title": "Multiply all",
            "description": "Create function that multiply all given numbers. Use loop when calculating. Create unit tests for function.",
            "severity": "Low",
            "priority": "Medium"
        },
        {
            "id": "FR-003",
            "title": "Unit tests",
            "description": "Create create unit tests for convert_to_uppercase and reverse_string functions.",
            "severity": "Low",
            "priority": "Medium"
        }
    ]
}

# Create query
query_engine = index.as_query_engine()
output = query_engine.query(json.dumps(
    issues) + " You are software developer. Find all bug listed in json and create proposal how to fix them. Create code for all feature requests.")
print(output)
```
## Run issues.py ##
```
python issues.py
```
**Output**
Bug Fixes:
1. Uppercase don't work: The bug is that the convert_to_uppercase function is actually converting the string to lowercase instead of uppercase. To fix this, we need to change the return statement in the convert_to_uppercase function to return input_string.upper() instead of input_string.lower().

2. Incorrect return value when reverting string: The bug is that the reverse_string function is not correctly reversing the string. To fix this, we need to change the return statement in the reverse_string function to return input_string[::-1] instead of input_string[::-2].

3. Incorrect return value when summing two numbers: The bug is that the sum_two_numbers function is actually subtracting the second number from the first number instead of summing them. To fix this, we need to change the return statement in the sum_two_numbers function to return num1 + num2 instead of num1 - num2.

4. Incorrect return value when subtracting two numbers: The bug is that the subtract_numbers function is actually adding the second number to the first number instead of subtracting them. To fix this, we need to change the return statement in the subtract_numbers function to return num1 - num2 instead of num1 + num2.

5. Incorrect return value when multiplying two numbers: The bug is that the multiply_numbers function is actually multiplying the first number with itself instead of multiplying the two numbers. To fix this, we need to change the return statement in the multiply_numbers function to return num1 * num2 instead of num1 * num1.

6. Incorrect return value when dividing two numbers: The bug is that the divide_numbers function is actually dividing the second number by the first number instead of dividing the first number by the second number. To fix this, we need to change the return statement in the divide_numbers function to return num1 / num2 instead of num2 / num1.

7. Summing all numbers don't work as expected: The bug is that the sum_all_numbers function is always returning one number bigger than it should be. To fix this, we need to change the return statement in the sum_all_numbers function to return sum(numbers) instead of sum(numbers) + 1.

8. Subtracting all numbers don't work as expected: The bug is that the subtract_all_numbers function is not correctly subtracting all the numbers. To fix this, we need to change the return statement in the subtract_all_numbers function to subtract each number from the initial value one by one using a loop.

Bug Fixes Code:
```python
# Fixing the convert_to_uppercase function
def convert_to_uppercase(input_string):
    return input_string.upper()

# Fixing the reverse_string function
def reverse_string(input_string):
    return input_string[::-1]

# Fixing the sum_two_numbers function
def sum_two_numbers(num1, num2):
    return num1 + num2

# Fixing the subtract_numbers function
def subtract_numbers(num1, num2):
    return num1 - num2

# Fixing the multiply_numbers function
def multiply_numbers(num1, num2):
    return num1 * num2

# Fixing the divide_numbers function
def divide_numbers(num1, num2):
    return num1 / num2

# Fixing the sum_all_numbers function
def sum_all_numbers(*numbers):
    return sum(numbers)

# Fixing the subtract_all_numbers function
def subtract_all_numbers(initial_value, *numbers):
    for num in numbers:
        initial_value -= num
    return initial_value
```

Feature Requests:
1. ROT13 cipher: To implement the ROT13 cipher, we can create a new function called apply_rot13_cipher that takes a string as input and applies the ROT13 cipher to it. We can then return the transformed string. Additionally, we should create unit tests for this function.

2. Divide all: To implement the function that divides all given numbers, we can create a new function called divide_all_numbers that takes any number of arguments and uses a loop to divide each number by the previous one. We can then return the final result. Additionally, we should create unit tests for this function.

3. Multiply all: To implement the function that multiplies all given numbers, we can create a new function called multiply_all_numbers that takes any number of arguments and uses a loop to multiply each number by the previous one. We can then return the final result. Additionally, we should create unit tests for this function.

4. Unit tests: To create unit tests for the convert_to_uppercase and reverse_string functions, we can use a testing framework like pytest. We can write test cases to verify that the functions are working correctly and producing the expected output.

Feature Requests Code:
```python
# Implementing the ROT13 cipher function
def apply_rot13_cipher(input_string):
    # Implementation of ROT13 cipher
    return transformed_string

# Implementing the divide_all_numbers function
def divide_all_numbers(*numbers):
    result = numbers[0]
    for num in numbers[1:]:
        result /= num
    return result

# Implementing the multiply_all_numbers function
def multiply_all_numbers(*numbers):
    result = numbers[0]
    for num in numbers[1:]:
        result *= num
    return result

# Writing unit tests for convert_to_uppercase and reverse_string functions
import pytest

def test_convert_to_uppercase():
    assert convert_to_uppercase("Hello World!") == "HELLO WORLD!"

def test_reverse_string():
    assert reverse_string("Hello World!") == "!dlroW olleH"
```
Note: The code provided above is just a template and may require further modifications based on the actual implementation and requirements.
## Notes ##
- Indexing should be tested with much larger code base, preferredly with real-world code
- Bugs and new features should more complex and also preferredly real-world issues
- LlamaIndex uses GPT-3.5 Turbo model by default, use of GPT-4 or other non-OpenAI model should be investigated.
- Results vary between each run. With multiple runs, an AI that doesn't find obvious bugs in number functions. This may be cause of used model and inaccurate prompt.
