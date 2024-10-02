Write a JavaScript function to check if a number is prime
  Update the function to include error handling for non-integer and non-positive inputs

Can you help me create a simple Flask application in Python that includes one API endpoint? This endpoint should handle GET requests at the URL /multiply, which accepts two query parameters a and b and returns the multiplication of these two parameters as a JSON response. Make sure to include error handling if the parameters are not provided or if they are not convertible to integers

Help me find errors in this code: def calculate_average(numbers): total_sum = sum(numbers) count = len(numbers) average = total_sum / count return average

Please write a Python function named calculate_area that takes an argument radius. The function should calculate the area of a circle given the radius. Ensure that the function handles non-numeric inputs by raising a ValueError with the message 'Input must be a numeric value'. Include comments in the code explaining each step.

Write a Python function using the request library to download a file from a URL and save the file to disk without using third-party libraries like wget
  Please add detailed comments that explain this code. Highlight any complex parts that might require deeper explanation

Write a Python function to calculate a factorial
  As my Python mentor, please write a function to calculate a factorial and explain how it works

As a beginner Python tutor, explain how to create a list in Python and add elements to it

You are a friendly coding mentor. Explain how to create a list in Python and add elements to it

As a friendly code guide, please explain how loops work in Python


```
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
As a software tester, analyze this function for potential edge cases and suggest robust handling strategies

def calculatediscount(price, discount):
  if discount > 100:
    raise ValueError('Discount cannot exceed 100%')
  return price * (100 - discount) / 100
# Test cases
print(calculatediscount(100, 105)) # Should raise an exception
```

“I’m working on an E-commerce application that uses a sqlite database as the backend and SQLAlchemy to execute queries. I’ll be working with a college intern on this code base in the upcoming summer break. Working as a friendly coding mentor for undergraduates, write clear documentation that will help the intern learn how the code works.