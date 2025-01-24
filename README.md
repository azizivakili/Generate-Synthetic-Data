# Comparing Traditional vs. AI-Based Synthetic Data Generation

This project, presents synthetic dataset generation using both OpenAI and traditional methods and at the end will compare the results.

This repository provides Python scripts to generate synthetic employee data for an e-commerce company using two methods:
* Traditional Method 
* OpenAI GPT Model

---

## Table of Contents  
- [Installation](#installation)  
- [Methods Overview](#methods-overview)  
  - [Traditional Method](#traditional-method)  
  - [OpenAI GPT Model](#openai-gpt-model)  
- [How to Run](#how-to-run)  
- [Output Files](#output-files)  
- [Customization](#customization)  
- [License](#license)  

---



## Installation

   ###  Clone the repository:

```
git clone https://github.com/<your-repo>/synthetic-data-generator.git
```
```
cd synthetic-data-generator
 ```

Install the required Python libraries:

```
pip install pandas numpy faker openai
 ```

Set up OpenAI API Key (required for the GPT Model):


## Methods Overview
### Traditional Method

The Traditional Method generates synthetic employee data using the Faker library and numpy to generate random names, job titles, hire dates, and salaries. This method is simple and doesn’t require an external API.

  ```
from faker import Faker
import numpy as np
import pandas as pd

fake = Faker()
num_records = 1000  # Change this to generate a different number of records
data = []

for _ in range(num_records):
    record = {
        "Name": fake.name(),
        "Job Title": fake.job(),
        "Hire Date": fake.date_between(start_date='-10y', end_date='today'),
        "Salary": round(np.random.uniform(30000, 150000), 2)
    }
    data.append(record)

df = pd.DataFrame(data)
df.to_csv("synthetic_employees.csv", index=False)
```

### OpenAI GPT Model

The OpenAI GPT Model approach uses OpenAI's GPT model to generate synthetic employee data based on a structured prompt. This method is more flexible and allows for detailed and realistic data generation.
Generate data using the OpenAI GPT model based on a customizable prompt.
Outputs data in JSON format, which is then converted to a DataFrame.
Requires an OpenAI API key and internet connection.

```
import openai
import pandas as pd
import json

# Set up your OpenAI API key
openai.api_key = '<YOU API KEY>'

def clean(dict_variable):
    return next(iter(dict_variable.values()))

prompt = """
Generate synthetic employee data for an e-commerce company. Include the following fields:
- Name (e.g., John Doe)
- Job Title (e.g., Software Engineer, Marketing Manager)
- Hire Date (e.g., YYYY-MM-DD format)
- Salary write the salary in capital S (e.g., $50,000 - $150,000)

Create data for 30 employees and output the result in JSON format.
"""

response = openai.ChatCompletion.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": prompt}]
)


# Parse the response to extract JSON content

customer_data = json.loads(response.choices[0].message.content)

# Print the raw JSON data (optional)
print(json.dumps(customer_data, indent=2))

# Convert to DataFrame
df_customers = pd.DataFrame(clean(customer_data))
print(df_customers)

#print to CSV file
df_customers.to_csv("synthetic_employees-AI-2.csv", index=False)

```

## How to Run

### 1. Run the Traditional Method:

To generate synthetic data using the Traditional Method, save the code as   traditional_method.py  and run:

```
python traditional_method.py
```

### 2. Run the OpenAI GPT Model:

To generate synthetic data using the OpenAI GPT Model, save the code as openai_model.py and run:

```
python openai_model.py
```

### Output Files

    synthetic_employees.csv:
        Generated using the Traditional Method.
        Contains employee data with 1000 rows.

    synthetic_employees-AI-2.csv:
        Generated using the OpenAI GPT Model.
        Contains employee data with 30 rows.

### Customization

#### Traditional Method:
Number of Records: Change the num_records variable in the script.
Salary Range: Adjust the np.random.uniform(30000, 150000) function to change the salary range.

#### OpenAI GPT Model:
Prompt Customization: Modify the prompt variable to change the schema or data size.
Employee Count: Change the number of employees generated by modifying the prompt text (e.g., "Create data for 50 employees").

## License

This project is licensed under the MIT License. You are free to use, modify, and distribute it as per the terms of the license.
