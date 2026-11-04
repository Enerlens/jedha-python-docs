# Python fundamentals 🐍

## What you will learn in this course 🧐🧐

This is your first real encounter with Python. You may have seen Python code before. But this is the first time you will write it and run it yourself. This notebook teaches the building blocks of Python programming. Every concept connects to a real task: processing article metadata from the Springer Nature scientific catalog. By the end, you will have the skills to store data, make decisions, repeat actions, package logic into functions, organize records, and handle errors. These are the exact skills you need to build data pipelines in a professional setting.

By the end of this notebook, you will be able to:

- Identify Python's core data types (string, integer, float, boolean) and explain when to use each one
- Write variables, f-strings
- Apply comparison operators and conditional statements (`if`, `elif`, `else`) to classify data based on business rules
- Construct `for` loops, `while` loops
- Design reusable functions with parameters, return values, default arguments, and docstrings
- Organize data using lists, dictionaries, and tuples, and choose the right structure for each situation

Optionally:
- Implement `try/except` error handling to build pipelines that continue to run when they receive bad data, instead of crashing
- Construct control flow tools (`break`, `continue`) to process collections of records
- Use string methods to store and format real article metadata

<Note type="tip" title="A concrete example">

To illustrate Python fundamentals, we will build a simple program that processes article metadata. This program is a small version of the larger data pipeline you will build later in the course.

Each concept applies to many other situations. But connecting them to a concrete example makes them easier to understand and remember.

</Note>

## Variables

The most basic instruction in any program is storing a piece of data. A **variable** is a name that points to a value in your computer's memory. Think of it as a labeled box. The label is the variable name. The contents are the value. You use the label whenever you need the value back.

You create a variable with a single `=` sign. The name goes on the left. The value goes on the right.

The next code block stores seven pieces of metadata about one scientific article from Springer Nature.


```python
# Store metadata about a Springer Nature article
# Lines starting with "#" are comments. Python ignores them.
# Comments explain what the code does for human readers.

article_title = "Transformer architectures for protein folding prediction"
authors = "Zhang Wei; Müller Hanna"
subject = "Computational Biology"
publication_year = 2024
citations = 142
pages = 18
open_access = True
```

Seven variables now hold information about one scientific article. Python stored each value in memory and attached the name you chose to it.

`article_title` holds the text of the title. `publication_year` holds the number 2024. `open_access` holds the value `True`, which means the article is free to read.

Use the `print()` function to display a variable's contents. `print()` is a **function**. A function is a named action. You write the function name, open the parentheses, put the input inside, and close the parentheses. Python then performs the action.


```python
print(article_title)
print(citations)
print(open_access)
```

`print(article_title)` displays the text stored in `article_title`. `print(citations)` displays the number 142. `print(open_access)` displays `True`. Each `print()` call produces one line of output.

### Data types

Not all values are the same kind. Python separates values into different **data types**. The four you use most often:

- **String** (`str`) stores text. Anything between quotes is a string. Article titles, author names, and DOI identifiers are all strings.

- **Integer** (`int`) stores whole numbers with no decimal point. Citation counts, page numbers, and publication years are integers.

- **Float** (`float`) stores decimal numbers. Impact factors, percentages, and ratios are floats.

- **Boolean** (`bool`) stores exactly `True` or `False`. Open access status and review completion flags are booleans.

The `type()` function returns the type of a value. The next code block checks the type of several variables.


```python
print(type(article_title))    # str
print(type(citations))        # int
print(type(open_access))      # bool

# Floats store decimal numbers
impact_factor = 69.504
print(type(impact_factor))    # float
```

`type(article_title)` returns `<class 'str'>` because the title is text. `type(citations)` returns `<class 'int'>` because 142 is a whole number. `type(open_access)` returns `<class 'bool'>` because `True` is a boolean. `type(impact_factor)` returns `<class 'float'>` because 69.504 has a decimal point.

Types matter because Python treats each type differently. You can add two numbers and get a sum. You can add two strings and get a longer string. But adding a number to a string produces an error. Understanding types now helps you avoid errors later that would be difficult to diagnose.

### f-strings

The professional way to format output in Python is the **f-string**. Place an `f` before the opening quote. Then insert variables directly inside curly braces `{}`. Python replaces each `{}` with the variable's value.

The next code block uses f-strings to produce clean, readable output.


```python
# f-strings embed variables directly in text
print(f"Title: {article_title}")
print(f"Citations: {citations}")

# Calculate citations per page, then format
citations_per_page = citations / pages
print(f"Citations per page: {citations_per_page:.1f}")
```

`f"Title: {article_title}"` inserts the value of `article_title` into the string. The result is `Title: Transformer architectures for protein folding prediction`.

`citations / pages` divides 142 by 18. The result is 7.888... The `:.1f` inside the braces rounds the number to one decimal place. So the output shows `7.9`.

f-strings replace the older approach of joining strings together with `+`. Use f-strings whenever you need to format output.

### Type conversion

The Springer Nature API returns everything as text, including numbers. A citation count of `"142"` is a string. You cannot perform arithmetic on it until you convert it to a number. Python provides `int()`, `float()`, `str()`, and `bool()` for conversion.

The next code block shows what happens when you convert a string to an integer.


```python
# Data arrives from the API as text
citations_text = "142"
print(type(citations_text))   # str

# Convert to integer for calculations
citations_number = int(citations_text)
print(type(citations_number)) # int

# Math works now
result = citations_number + 10
print(result)                 # 152
```

`citations_text` is a string. It looks like a number, but Python treats it as text. `int(citations_text)` reads the text `"142"` and creates the integer `142`. Now you can do math with it. `citations_number + 10` produces `152`.

<Note type="important" title="API data is always text">

When you connect to the Springer Nature API in a later module, every value in the response arrives as a string. Citation counts, page numbers, and dates all arrive as text. Your pipeline must convert each field to the correct type. Practice this conversion now so it becomes automatic.

</Note>

## Comparisons and conditional execution

Now that you know how to store and change data, you need a way to make decisions based on that data. A **comparison** asks a question and returns a boolean: `True` or `False`. Python provides six comparison operators.

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| `==` | Equal to | `5 == 5` | `True` |
| `!=` | Not equal to | `5 != 3` | `True` |
| `>` | Greater than | `5 > 3` | `True` |
| `<` | Less than | `5 < 3` | `False` |
| `>=` | Greater or equal | `5 >= 5` | `True` |
| `<=` | Less or equal | `3 <= 5` | `True` |

<Note type="important" title="= versus ==">

A single `=` means **assignment**, which stores a value. A double `==` means **comparison**, which checks equality. `citations = 100` stores the number. `citations == 100` asks "is citations equal to 100?" and returns True or False. Confusing these two operators is the most common beginner mistake.

</Note>

The next code block uses comparison operators and the logical keywords `and`, `or`, and `not` to evaluate article metadata.


```python
citations = 142
open_access = True

print(citations > 100)       # True
print(citations == 200)      # False
print(open_access != False)  # True

# Combine conditions with 'and', 'or', 'not'
high_impact_oa = citations > 100 and open_access
print(f"High-impact open access: {high_impact_oa}")
```

`citations > 100` asks "is 142 greater than 100?" The answer is `True`. `citations == 200` asks "is 142 equal to 200?" The answer is `False`. `open_access != False` asks "is True not equal to False?" The answer is `True`.

`citations > 100 and open_access` combines two conditions with `and`. Both must be True for the result to be True. Since 142 is greater than 100 *and* the article is open access, the result is `True`.

Comparisons produce True or False. **Conditional statements** use that result to choose which code runs. `if` checks a condition. When the condition is True, Python runs the indented block below it. When the condition is False, Python checks the `elif` condition (short for "else if") as an alternative. If no condition matches, Python runs the `else` block as the fallback.

<Note type="important" title="Indentation matters">

**Indentation matters.** Python uses indentation (four spaces) to define blocks. Wrong indentation produces an error.

</Note>

The next code block assigns a priority level based on citation count.


```python
# Assign a priority level based on citation count
citations = 142

if citations >= 200:
    priority = "High"
else:
    priority = "Medium"

print(f"Article priority: {priority}")
```

`citations >= 200` checks if 142 is greater than or equal to 200. It is not. So Python skips the `if` block and runs the `else` block instead. The variable `priority` gets the value `"Medium"`.

The first condition that evaluates to True determines which block runs. If no conditions are True, the `else` block runs. You can have as many `elif` blocks as you need. However, you can have only one `else` block, and it must come at the end.

Also, Python runs the block of the first condition that matches and skips the rest. This means that the order of the conditions matters. Put the most specific conditions first and the most general conditions last.

The next code block uses `elif` to create multiple priority levels.


```python
citations = 142

if citations >= 200:
    priority = "critical"
elif citations >= 100:
    priority = "high"
elif citations >= 50:
    priority = "medium"
else:
    priority = "low"

print(f"Priority: {priority}")
```

Python checks each condition from top to bottom. `142 >= 200` is False, so Python moves to the next condition. `142 >= 100` is True, so `priority` becomes `"high"`. Python stops checking and skips the remaining `elif` and `else` blocks.

The next code block shows a more realistic pipeline classification. It combines `and` with `if/elif/else` to route articles into different processing queues.


```python
# Classify an article into a processing queue
article_title = "Transformer architectures for protein folding prediction"
citations = 142
open_access = True

if citations >= 100 and open_access:
    queue = "daily"
    truncated_title = article_title[:40]
    print(f"'{truncated_title}...' → daily processing queue")
elif citations >= 50:
    queue = "weekly"
    truncated_title = article_title[:40]
    print(f"'{truncated_title}...' → weekly processing queue")
else:
    queue = "monthly"
    truncated_title = article_title[:40]
    print(f"'{truncated_title}...' → monthly processing queue")
```

The first condition checks two things at once: `citations >= 100 and open_access`. Both are True for this article. So the article goes into the `"daily"` queue. `article_title[:40]` takes only the first 40 characters of the title. This keeps the output short and readable.

<Note type="tip">

Handle the **most restrictive condition first**. Check `>= 200` before `>= 100` before `>= 50`. If you check `>= 50` first, an article with 200 citations matches immediately and never reaches the stricter checks.

</Note>

## Loops

### For loops

In a real pipeline, you do not process one article at a time. You process thousands or millions. Processing many items requires **loops**. A `for` loop takes a collection and runs the same code once for each item.

The next code block loops through a list of subject names and prints each one.


```python
# subjects is a list of strings
subjects = ["Computational Biology", "Earth Sciences", "Quantum Physics","Genetics", "Neuroscience"]

# Process each subject one by one
for subject in subjects:
    print(f"Processing articles in: {subject}")
```

Python ran `print()` five times, once per subject. On each pass, the variable `subject` held a different value from the list. First `"Computational Biology"`, then `"Earth Sciences"`, and so on.

Loops become much more useful when you combine them with conditional statements. The next code block classifies each citation count as `"HIGH"` or `"standard"`.


```python
citation_counts = [142, 89, 201, 67, 53, 178, 34, 312]

for count in citation_counts:
    if count >= 100:
        level = "HIGH"
    else:
        level = "standard"
    print(f"  Citations: {count:>4} → {level}")
```

For each number in `citation_counts`, the loop checks if it is 100 or more. If yes, `level` becomes `"HIGH"`. If no, `level` becomes `"standard"`. The `{count:>4}` in the f-string right-aligns the number in a space 4 characters wide. This makes the output align neatly in columns.

### range() and enumerate()

`range()` generates a sequence of numbers. `range(5)` produces 0, 1, 2, 3, 4. `range(0, 50, 10)` produces 0, 10, 20, 30, 40. The third number is the **step size**, which means how much to increase each time.

`enumerate()` pairs each item with its position. This is useful for numbering and error tracking.

The next code block simulates fetching articles from the API in batches of 10.


```python
# Simulate fetching articles in batches from the API
for start in range(0, 50, 10):
    first_article = start + 1
    last_article = start + 10
    print(f"Fetching articles {first_article} to {last_article}...")
```


```python
# enumerate() gives you position + value
subjects = ["Computational Biology", "Earth Sciences", "Quantum Physics"]

for index, subject in enumerate(subjects):
    print(f"  [{index}] {subject}")
```

`range(0, 50, 10)` produces the numbers 0, 10, 20, 30, 40. The loop runs five times. Each time, `start` holds the next number. The code adds 1 and 10 to calculate the human-readable batch range.

`enumerate(subjects)` pairs each subject with its position. On the first pass, `index` is 0 and `subject` is `"Computational Biology"`. On the second pass, `index` is 1 and `subject` is `"Earth Sciences"`. This is simpler than maintaining a separate counter variable yourself.

### While loops

A `while` loop **repeats as long as a condition stays True**. Use it when you do not know in advance how many times you need to repeat. A common example is paginating through API results until no more pages exist.

The next code block simulates fetching pages from an API.


```python
current_page = 1
total_pages = 5

while current_page <= total_pages:
    print(f"Fetching page {current_page} of {total_pages}...")
    current_page += 1  # Without this line, the loop runs forever

print("All pages retrieved.")
```

The loop starts with `current_page = 1`. Each time, it prints the current page and then adds 1 with `current_page += 1`. When `current_page` reaches 6, the condition `6 <= 5` is False. The loop stops. Python then runs the line after the loop.

<Note type="important" title="Avoid infinite loops">

A `while` loop runs forever if its condition never becomes False. Always make sure something inside the loop changes the condition. Forgetting `current_page += 1` above creates an **infinite loop**. If this happens, click the stop button in the toolbar.

</Note>


<Note type="tip" title="When to use for vs while">

Use `for` when you know how many times to repeat (for example, processing a list of articles). Use `while` when you repeat until a condition changes (for example, paginating until no more pages exist).

</Note>

## Functions

So far, every piece of code you wrote ran from top to bottom, once. That works for small scripts. However, real pipelines repeat the same logic in many places. For example, a pipeline may classify an article in one place, classify it again in another place, and classify it a third time further down in the code. Copying and pasting the same block of code is fragile. If you change the rule in one place and forget to change the others, your pipeline behaves inconsistently.

A **function** solves this problem. It is a named, reusable block of code that performs one specific task. You write the logic once, give it a name, and call that name whenever you need the task done. Every call runs exactly the same logic.

This principle is called **DRY**: *Don't Repeat Yourself*. Functions are the main tool for applying this principle.

Think of a function like a machine in a factory. You put raw material in (**inputs**). The machine processes it. A finished product comes out (**output**). The machine works the same way regardless of who provides the material or what happens to the product afterward. You can run the same machine a thousand times without rebuilding it each time.

In Python, the inputs are called **parameters** (or **arguments** when you call the function). The output is the **return value**.

Functions also make your code easier to read, test, and fix. If the classification rule changes, you update one function. Everything that calls it automatically uses the new rule.

A **function** is a reusable block of code that performs one specific task. You define it once with `def`. Then you call it whenever you need it.

**Parameters** are the inputs of the function. The `return` statement sends the output back to the caller.

<img src="https://ai-essentials-assets.s3.eu-west-3.amazonaws.com/M01-Python_for_AI/AIE-M01-D01-What_is_a_function.png" />

Here is how to define a function:

```python
def function_name(parameter1, parameter2):
    # code to execute
    return result
```

Here is how to call a function:

```python
result = function_name(argument1, argument2)
```

The next code block defines a function that classifies an article based on its citation count.



```python
# Define a function to classify article impact
def classify_impact(citations):
    """Classify article impact based on citation count.

    Parameters:
        citations: integer count of citations

    Returns:
        string: "high", "medium", or "low"
    """
    if citations >= 100:
        return "high"
    elif citations >= 50:
        return "medium"
    else:
        return "low"

# Call the function with different citation counts
print(classify_impact(142))   # high
print(classify_impact(67))    # medium
print(classify_impact(34))    # low
```

`def classify_impact(citations):` creates a function named `classify_impact` that takes one input called `citations`. Inside, the `if/elif/else` block returns a different string depending on the value. `return` sends the result back to the code that called the function.

The triple-quoted text after `def` is a **docstring**. It describes what the function does. Professional Python code always includes docstrings.

Functions can call other functions. This is how you build complex systems from simple pieces. The next code block defines a function that calls `classify_impact` inside it.


```python
def format_article_log(title, citations, open_access):
    """Create a formatted log entry for pipeline output."""
    impact = classify_impact(citations)
    impact_upper = impact

    if open_access:
        access_tag = "OA"
    else:
        access_tag = "CLOSED"

    log_entry = f"[{impact_upper}] [{access_tag}] {title} ({citations} cit.)"
    return log_entry


result = format_article_log(
    "Transformer architectures for protein folding prediction",
    142, True
)
print(result)
```

`format_article_log` takes three inputs: title, citations, and open_access. It calls `classify_impact(citations)` to get the impact level. The `if/else` block selects the access tag. Finally, the f-string builds the full log entry and `return` sends it back to the caller.

This pattern of building small functions and combining them is the foundation of maintainable code. Each function does one thing. You can test and fix each one separately.

### Default parameter values

Some parameters have a sensible default value. In that case, the caller can omit them when calling the function. You set a default value with `=` in the function definition. The next code block shows a search function with optional parameters.


```python
def search_articles(subject, min_citations=0, open_access_only=False):
    """Simulate a Springer Nature catalog search."""
    query = f"Searching for '{subject}'"

    if min_citations > 0:
        query += f" with ≥{min_citations} citations"

    if open_access_only:
        query += " (open access only)"

    print(query)


# Call with just the required argument
search_articles("Neuroscience")

# Override one default
search_articles("Neuroscience", min_citations=50)

# Override both defaults
search_articles("Neuroscience", min_citations=50, open_access_only=True)
```

`min_citations=0` and `open_access_only=False` are default values. If you do not pass them when calling the function, Python uses the defaults. The first call uses only `"Neuroscience"`. The second call overrides `min_citations` to 50. The third call overrides both. Using named arguments like `min_citations=50` makes the code clear and readable.

## Data structures

Most of the time, you work with collections of data, not single values. Python provides powerful **data structures** to organize and manage groups of related data.

The most common ones are **lists** (ordered collections), **dictionaries** (key-value pairs), and **tuples** (fixed, unchangeable collections). DataFrames are covered in a later lecture.

### Lists

A **list** stores multiple items in order. You create it with square brackets `[]`. Lists can hold any type.

The next code block creates a list and accesses items by position.


```python
subjects = ["Computational Biology", "Earth Sciences", "Quantum Physics",
            "Genetics", "Neuroscience"]

# Access items by position (index starts at 0)
first_subject = subjects[0]
print(first_subject)          # First item

last_subject = subjects[-1]
print(last_subject)           # Last item

middle_slice = subjects[1:3]
print(middle_slice)           # Slice: items at index 1 and 2

total_subjects = len(subjects)
print(total_subjects)         # Number of items: 5
```

`subjects[0]` returns the first item. Python counts from 0, not 1. `subjects[-1]` returns the last item. Negative indexes count from the end. `subjects[1:3]` returns a **slice**, which contains the items at positions 1 and 2 (the end index is excluded). `len(subjects)` returns the number of items in the list.

**Lists** are mutable: it is possible to update an element by accessing it thanks to its index:


```python
subjects[3] = "Biology"
print(subjects)
```

### Dictionaries

A **dictionary** stores data as **key-value pairs**. Instead of accessing items by position, you access them by name. This works like a printed dictionary, where you look up a word (the key) to find its definition (the value).

Create a dictionary with curly braces `{}` using the pattern `key: value`.

For example, you can store article metadata in a dictionary:

```python
article = {
    "title": "The impact of AI on scientific publishing",
    "authors": ["Alice Smith", "Bob Jones"],
    "year": 2024,
    "citations": 142,
    "open_access": True
}
```

The keys are strings that describe the data. The values can be any type. `"title"` is the key and `"The impact of AI on scientific publishing"` is the value. You access values by their keys: `article["title"]` returns the title string.

The next code block creates a dictionary for one article and reads two fields from it.


```python
# One article as a dictionary. All metadata in one place.
article = {
    "article_id": "SN-001",
    "title": "Transformer architectures for protein folding prediction",
    "authors": "Zhang Wei; Müller Hanna",
    "subject": "Computational Biology",
    "citations": 142,
    "pages": 18,
    "open_access": True
}

print(article["title"])
print(article["citations"])
```

Compare this to storing the same data in separate variables or parallel lists. All fields for one article are now stored in a single structure. You access them by name, not by remembering index positions. `article["citations"]` is self-documenting, which means that anyone reading this code understands what the value represents.

#### Modifying dictionaries

Dictionaries are mutable. You can add new keys, update existing values, and safely access keys that might not exist. The next code block demonstrates all three.


```python
# Add a new field
impact_level = classify_impact(article["citations"])
article["impact_level"] = impact_level
print(f"Impact: {article['impact_level']}")

# Update an existing value
article["citations"] = 157
print(f"Updated citations: {article['citations']}")

# Safe access with .get(). Returns default if key is missing.
abstract = article.get("abstract", "No abstract available")
print(f"Abstract: {abstract}")
```

`article["impact_level"] = impact_level` adds a new key to the dictionary. `article["citations"] = 157` overwrites the old value (142) with the new one (157). `article.get("abstract", "No abstract available")` tries to find the key `"abstract"`. Since it does not exist, it returns the default value instead of crashing.

<Note type="important" title="Always use .get() for external data">

Accessing a missing key with `article["abstract"]` crashes your program with a `KeyError`. The `.get()` method returns a default value instead. Use `.get()` whenever you process data from APIs or files. Fields that existed yesterday might be missing today.

</Note>

#### Lists of dictionaries

The most common pattern in data engineering is a **list of dictionaries**. Each dictionary is one record. The list holds all records. This is exactly how the Springer Nature API returns search results.

The next code block processes a list of article dictionaries and prints a formatted summary for each one.


```python
articles = [
    {"article_id": "SN-001", "title": "Transformer architectures",
     "citations": 142, "open_access": True},
    {"article_id": "SN-002", "title": "Climate feedback loops",
     "citations": 89, "open_access": False},
    {"article_id": "SN-003", "title": "Quantum error correction",
     "citations": 201, "open_access": True},
    {"article_id": "SN-010", "title": "Large language models for theorem proving",
     "citations": 312, "open_access": True}
]

# Process with clean, readable code
for article in articles:
    impact = classify_impact(article["citations"])

    if article["open_access"]:
        access = "OA"
    else:
        access = "CLOSED"

    title = article["title"]
    cit = article["citations"]
    print(f"  [{access}] [{impact}] {title} — {cit} cit.")
```

There are no more parallel lists and no more index tracking. Each article is self-contained. The loop reads almost like an English sentence: "for each article in articles, classify it and print it."

### Tuples

A **tuple** looks like a list but uses parentheses. The critical difference is that tuples are **immutable**. Once a tuple is created, you cannot change it. Use tuples for data that must stay fixed. Configuration values, API endpoints, and fixed records are good examples.

The next code block creates a tuple and shows what happens when you try to change it.


```python
# Fixed API configuration. Should never change at runtime.
api_config = ("https://api.springernature.com/meta/v2", "json", 25)

# Unpack into separate variables
endpoint, format_type, page_size = api_config
print(f"Endpoint: {endpoint}")
print(f"Page size: {page_size}")

# Attempting to modify raises an error
try:
    api_config[2] = 50
except TypeError as e:
    print(f"Error: {e}")
```

`api_config` is a tuple with three items. `endpoint, format_type, page_size = api_config` **unpacks** the tuple. It assigns each item to a separate variable in one line. This only works if the number of variables matches the number of items.

`api_config[2] = 50` tries to change the third item. Python raises a `TypeError` because tuples do not allow changes. This is the purpose of tuples. They protect data from accidental modification.

## Optional - Other useful features

### String methods

Strings have built-in actions called **methods**. You call a method with a dot after the string variable. Methods return a *new* string. The original stays unchanged.

Here are some useful string methods:
- `lower()` converts to lowercase,
- `upper()` converts to uppercase,
- `title()` converts to title case,
- `strip()` removes extra spaces from the start and end,
- `replace(old, new)` replaces text,
- `split(sep)` splits a string into a list,
- `join(list)` joins a list into one string,
- `startswith(prefix)` and `endswith(suffix)` check the beginning or end of a string,
- `find(sub)` returns the position of text or -1 if not found, and
- `count(sub)` counts how many times text appears.

The next code block demonstrates several of these methods on a subject name.


```python
subject = "Computational Biology"

# Each method returns a NEW string
upper_subject = subject.upper()
print(upper_subject)                       # COMPUTATIONAL BIOLOGY

lower_subject = subject.lower()
print(lower_subject)                       # computational biology

title_subject = subject.title()
print(title_subject)                       # Computational Biology

starts_with_comp = subject.startswith("Comp")
print(starts_with_comp)                    # True

ends_with_logy = subject.endswith("logy")
print(ends_with_logy)                      # True

position_of_bio = subject.find("Bio")
print(position_of_bio)                     # 14

count_of_o = subject.count("o")
print(count_of_o)                          # 4
```

`subject.upper()` creates a new string where every letter is uppercase. The original `subject` still holds `"Computational Biology"`. `subject.find("Bio")` returns 14 because the letter `B` in `Bio` is at position 14 (counting from 0). `subject.count("o")` counts every `o` in the string and returns 4.

The next two code blocks show `strip()`, `replace()`, `split()`, and `join()` in action.


```python
# strip() removes leading and trailing whitespace
messy_title = "  Transformer Architectures  "
clean_title = messy_title.strip()
print(f"'{messy_title}' → '{clean_title}'")
```


```python
# replace() swaps one piece of text for another
new_title = clean_title.replace("Architectures", "Models")
print(new_title)                       # Transformer Models
```


```python
# split() breaks a string into a list of pieces
words = new_title.split(" ")
print(words)                           # ['Transformer', 'Models']
```


```python
# join() combines a list back into one string
joined = " - ".join(words)
print(joined)                          # Transformer - Models
```

`messy_title.strip()` removes the extra spaces at the start and end. The result is a clean string with no surrounding whitespace. `clean_title.replace("Architectures", "Models")` finds the word `"Architectures"` and replaces it with `"Models"`. `new_title.split(" ")` splits the string at every space and returns a list of words. `" - ".join(words)` takes the list `["Transformer", "Models"]` and joins the items into one string, with `" - "` between each word.

These methods become essential when your pipeline processes real data:
- Article titles arrive with extra spaces.
- Author names use inconsistent capitalization.
- DOIs include prefixes that you need to remove.

### Interrupting loops: `break` and `continue`
`break` exits the loop immediately. `continue` skips the current item and moves to the next. The next two code blocks demonstrate each one.


```python
# break: stop on first critical error
articles = ["valid", "valid", "RETRACTED", "valid"]

for article in articles:
    if article == "RETRACTED":
        print("Retracted article found. Stopping.")
        break
    print(f"Processing: {article}")
```


```python
# continue: skip invalid items without stopping
citation_counts = [142, 0, 89, 0, 201, 67]

for count in citation_counts:
    if count == 0:
        continue  # Skip zero-citation articles
    print(f"Processing article with {count} citations")
```

In the first loop, `break` stops the entire loop as soon as the loop reaches a retracted article. The fourth item (`"valid"`) is never reached.

In the second loop, `continue` skips only the current item. When `count` is 0, Python jumps back to the top of the loop and moves to the next number. The loop still processes all non-zero items.

💡 `break` and `continue` also work with `while` loops!

### Creating and modifying lists

Lists are **mutable**. That means you can add, remove, and change items after creation. The next code block demonstrates common list operations.


```python
# Build a processing queue by adding items one at a time
queue = []
queue.append("SN-001")
queue.append("SN-002")
queue.append("SN-003")
print(f"Queue: {queue}")

# Remove a specific item
queue.remove("SN-002")
print(f"After remove: {queue}")

# Sort and aggregate
counts = [142, 89, 201, 67, 312]
counts.sort(reverse=True)
print(f"Sorted: {counts}")

max_count = max(counts)
min_count = min(counts)
total = sum(counts)
average = total / len(counts)
print(f"Max: {max_count}, Min: {min_count}, Avg: {average:.1f}")
```

`queue.append("SN-001")` adds one item to the end of the list. `queue.remove("SN-002")` finds and removes the first occurrence of that value. `counts.sort(reverse=True)` sorts the list from largest to smallest. *This changes the list itself.* `max()`, `min()`, and `sum()` are built-in functions that work on any list of numbers.

#### Building new lists with loops

A very common task is creating a new list from an existing one. You loop through the original, apply a rule, and collect the results. The next code block extracts all citation counts from a list of articles, then filters only the open-access titles.


```python
articles = [
    {"title": "Transformer architectures for protein folding prediction",
     "citations": 142, "open_access": True},
    {"title": "Quantum entanglement in photosynthesis",
     "citations": 89, "open_access": False},
    {"title": "Earthquake prediction using machine learning",
     "citations": 201, "open_access": True},
    {"title": "Genetic basis of Alzheimer's disease",
     "citations": 67, "open_access": False},
    {"title": "Neural networks for brain-computer interfaces",
     "citations": 312, "open_access": True}
]

# Extract all citation counts into a new list
all_citations = []
for a in articles:
    count = a["citations"]
    all_citations.append(count)

print(f"All citations: {all_citations}")
```


```python
# Filter: only open-access article titles
oa_titles = []
for a in articles:
    if a["open_access"]:
        title = a["title"]
        oa_titles.append(title)

print(f"Open access: {oa_titles}")
```

The first loop starts with an empty list `all_citations`. For each article in the list, it reads the `"citations"` value and appends it. After five passes, the new list holds `[142, 89, 201, 67, 312]`.

The second loop adds a filter. It only appends titles where `a["open_access"]` is `True`. Three of the five articles are open access, so the result list has three titles. This pattern of "start empty, loop, conditionally append" is extremely common in data work.

### Error handling

When Python encounters a problem, it raises an **exception** (an error) and stops. In a data pipeline, one bad record out of ten thousand should not stop everything.

The `try/except` pattern catches exceptions and lets your program decide what to do instead of crashing.

To make debugging easier, `try/except` is often used with specific error types like `ValueError`, `TypeError`, `KeyError`, and others. These **[built-in exceptions](https://docs.python.org/3/library/exceptions.html)** cover common problems, so you can handle each one differently.

The next code block shows a basic `try/except` that catches a `ValueError`.


```python
# Without error handling, this would crash:
# result = int("not_a_number")  # ValueError!

# With error handling: graceful recovery
try:
    result = int("not_a_number")
    print(f"Converted: {result}")
except ValueError:
    print("Could not convert. Skipping this value.")

print("Pipeline continues running...")
```

Python tries to run the code inside the `try` block. `int("not_a_number")` fails because `"not_a_number"` is not a valid number. Python raises a `ValueError`. Instead of crashing, the `except ValueError` block runs. It prints a warning message. Then the program continues normally with the next line.

The next code block wraps this pattern into a reusable function.


```python
def safe_int(value, default=0):
    """Safely convert a value to integer.
    Returns default if conversion fails.
    """
    try:
        return int(value)
    except (ValueError, TypeError):
        return default

print(safe_int("142"))     # 142
print(safe_int("N/A"))     # 0
print(safe_int(None))      # 0
print(safe_int(""))         # 0
```

`safe_int` tries to convert any value to an integer. If the conversion fails with a `ValueError` (for example, text that is not a number) or a `TypeError` (for example, a wrong type such as `None`), the function returns the default value instead. This function handles four different kinds of bad input without crashing.

### Error handling in batch processing

Now, imagine you receive data from the [Springer Nature API](https://dev.springernature.com/). You want to process a batch of articles. But some records might have bad or missing fields. You do not want one bad record to crash the entire pipeline.

Combine `try/except` with loops to process entire batches. Log problems without stopping. The next code block demonstrates this pattern.


```python
def process_article(raw):
    """Process one raw article record safely."""
    try:
        title = raw.get("title", "Unknown")
        citations_raw = raw.get("citations", 0)
        citations_int = int(citations_raw)
        open_access_flag = raw.get("open_access", False)

        result = {
            "title": title,
            "citations": citations_int,
            "open_access": open_access_flag
        }
        return result

    except (ValueError, TypeError) as e:
        print(f"  WARNING: Skipping bad record. {e}")
        return None


# Data received with one bad record
raw_batch = [
    {"title": "Good article", "citations": "142", "open_access": True},
    # "N/A" will cause ValueError when converting to int
    {"title": "Bad citations", "citations": "N/A", "open_access": True},
    {"title": "Another good one", "citations": "89", "open_access": False}
]

processed = []
for raw in raw_batch:
    result = process_article(raw)
    if result is not None:
        processed.append(result)

print(f"\nProcessed {len(processed)} of {len(raw_batch)} articles")
for p in processed:
    title = p["title"]
    cit = p["citations"]
    print(f"  {title} — {cit} citations")
```

`process_article` tries to read each field from the raw dictionary. It uses `.get()` for safe access. It converts `citations` from a string to an integer with `int()`. If the conversion fails (for example, with `"N/A"`), the `except` block prints a warning and returns `None`.

The main loop calls `process_article` for every record. It checks if the result is `None`. If not, it appends the cleaned record. At the end, 2 out of 3 articles were processed. The bad record was logged but did not stop the pipeline.

<Note type="tip">

Always catch **specific** exception types. A bare `except:` catches everything, including logic bugs you need to see. Catch `ValueError` and `TypeError` when converting data. Catch `KeyError` when accessing dictionaries. Let unexpected exceptions appear, so that you can find and fix the root cause.

</Note>

## Resources 📚📚

- [docs.python.org](https://docs.python.org/3/tutorial/)
- [Introduction](https://docs.python.org/3/tutorial/introduction.html)
- [Control Flow](https://docs.python.org/3/tutorial/controlflow.html)
- [Data Structures](https://docs.python.org/3/tutorial/datastructures.html) [Errors and Exceptions](https://docs.python.org/3/tutorial/errors.html)
- [Variables](https://realpython.com/python-variables/)
- [Dictionaries](https://realpython.com/python-dicts/)
- [Exception Handling](https://realpython.com/python-exceptions/)
