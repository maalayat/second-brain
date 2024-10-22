## Roles
- A Seasoned software engineer
- A paranoid security expert
- A site reliability engineer

## Company context
- A company that suffers denial of service attacks
- A company that will have to quickly scale from thousands of users to billion of users

## Prompting Best Practices
- **Be specific:**  Providing detailed context about the problem
- **Assign a role:** Assigning roles to tailor the output you receive.
- **Request an expert opinion:** Assign an expert role and ask the LLM to evaluate the work you've already done to further refine it.
- **Give feedback:** Iteratively prompt the LLM and provide feedback on the output you receive to get closer to your expected results 

## Examples

```
Can you help me create a simple Flask application in Python that includes one API endpoint? This endpoint should handle GET requests at the URL /multiply, which accepts two query parameters a and b and returns the multiplication of these two parameters as a JSON response. Make sure to include error handling if the parameters are not provided or if they are not convertible to integers
```

```
Please write a Python function named «...» that takes an argument «...». The function should «...» given the «...». Ensure that the function handles non-numeric inputs by raising a ValueError with the message «». Include comments in the code explaining each step.
```

```
Write a Python function using the «...» library to download a file from a URL and save the file to disk without using third-party libraries like «...»

Please add detailed comments that explain this code. Highlight any complex parts that might require deeper explanation
```

```
As my Python mentor, please write a function to «...» and explain how it works
```

```
As a beginner Python tutor, explain how to create a «...»
```

```
⭐
As a beginner Rust tutor, explain me this code. Highlight any complex parts that might require deeper explanation
```

```
You are a friendly coding mentor. Explain how to create a «...»
```

```
As a friendly code guide, please explain how «...» work in Python
```

```
⭐
As both a software architect and a security expert, evaluate this Python script for a web application and suggest architectural improvements and security enhancements

def storeuserdata(user_data):
  database = open('user_database.txt', 'a')
  database.write(str(user_data))
  database.close()
  
storeuserdata({'username': 'admin', 'password': '1234'})

```

```
As a contributor to open-source Python projects, critique this Python library for data visualization and suggest enhancements to make it comparable to major libraries like Matplotlib or Seaborn

import matplotlib.pyplot as plt
class DataVisualizer:
  def init(self, data):
    self.data = data
  def plot(self, kind='line'):
    if kind == 'line':
      plt.plot(self.data)
    elif kind == 'bar':
      plt.bar(range(len(self.data)), self.data)
    plt.show()
#Example usage
visualizer = DataVisualizer([10, 20, 30, 40, 50])
visualizer.plot('bar')
```

```
As an NLP expert, suggest improvements to this text summarization feature to enhance its functionality and accuracy

import nltk
class TextAnalyzer:
  def init(self, text):
    self.text = text
  def summarize(self):
    sentences = nltk.senttokenize(self.text)
    return ' '.join(sentences[:2])
```

```
⭐⭐
As a software tester, analyze this function for potential edge cases and suggest robust handling strategies

def calculatediscount(price, discount):
  if discount > 100:
    raise ValueError('Discount cannot exceed 100%')
  return price * (100 - discount) / 100
# Test cases
print(calculatediscount(100, 105)) 
```

```
“I’m working on an E-commerce application that uses a sqlite database as the backend and SQLAlchemy to execute queries. I’ll be working with a college intern on this code base in the upcoming summer break. Working as a friendly coding mentor for undergraduates, write clear documentation that will help the intern learn how the code works.
```
```
Write some code to implement a linked list in python.
What are the downsides and overheads associated with linked lists compared to other python data structures?
```
```
Can you give me an example of where (3) could be a problem?
```
```
Take on the role of an expert software developer at a company that suffers from denial of service attacks. If I implement some routines with code like this, what risks am I facing?
```
```
How to would you modify the code to mitigate against these?
```

```
⭐⭐
As an expert software tester who is teaching a new person how to write test cases, can you please analyze this code, and provide a set of test cases, explaining each one:
```