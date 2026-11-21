# Practical – 05: Anonymization Techniques

## Title
**Implementation of Data Anonymization Techniques on a Student Dataset**

## 1. Aim

To understand and implement different data anonymization techniques, such as Data Masking, k-Anonymity, and Differential Privacy, using Python to protect sensitive personal information while maintaining data utility.

## 2. Objectives

The objectives of this practical are:

1. To understand the concept of data anonymization.
2. To identify personally identifiable information (PII) and sensitive attributes.
3. To implement data masking techniques.
4. To understand and implement k-anonymity.
5. To implement differential privacy using Laplace noise.
6. To compare original and anonymized datasets.
7. To understand the privacy-utility trade-off.
8. To study the advantages and limitations of anonymization techniques.

## 3. Introduction

Data anonymization is the process of modifying or transforming personal information so that individuals cannot be easily identified from a dataset.

Organizations collect personal information for research, education, healthcare, banking, marketing, and other purposes. Before sharing or analyzing such information, it is important to protect individuals' privacy.

Data anonymization helps prevent unauthorized identification and reduces the risk of exposing sensitive information.

### Basic Process

Original Dataset  
↓  
Identify Sensitive Information  
↓  
Apply Anonymization Techniques  
↓  
Generate Anonymized Dataset  
↓  
Secure Data Sharing and Analysis

## 4. What is Data Anonymization?

Data anonymization involves removing or modifying information that can identify an individual.

For example:

**Original Dataset**

| Name | Age | City | Marks |
|---|---:|---|---:|
| Rahul | 21 | Delhi | 85 |
| Priya | 22 | Delhi | 91 |

**After Anonymization**

| Age | City | Marks |
|---|---|---:|
| 18–25 | Delhi-NCR | 85 |
| 18–25 | Delhi-NCR | 91 |

The names have been removed, and the age and city have been generalized to reduce the possibility of identification.

## 5. Types of Data Attributes

Data attributes are classified into three major categories:

### 5.1 Direct Identifiers

Direct identifiers are attributes that can directly identify an individual.

Examples:
- Name
- Email address
- Phone number
- Aadhaar number
- Student ID

### 5.2 Quasi-Identifiers

Quasi-identifiers cannot necessarily identify a person individually, but combinations of these attributes may identify someone.

Examples:
- Age
- Gender
- City
- PIN code
- Education

### 5.3 Sensitive Attributes

Sensitive attributes contain private information about an individual.

Examples:
- Salary
- Medical information
- Financial records
- Examination marks
- Personal information

## 6. Dataset Used

For this practical, a sample student dataset is created using Python.

The dataset contains the following attributes:

| Attribute | Data Type | Category |
|---|---|---|
| Name | String | Direct Identifier |
| Email | String | Direct Identifier |
| Phone | String | Direct Identifier |
| Age | Integer | Quasi-Identifier |
| Gender | String | Quasi-Identifier |
| City | String | Quasi-Identifier |
| Course | String | Quasi-Identifier |
| Marks | Integer | Sensitive Attribute |

The dataset contains six sample student records with fictional information.

## 7. Technologies Used

**Programming Language:** Python

**Environment:** Google Colab / Jupyter Notebook

**Libraries Used:**
- Pandas
- NumPy
- Matplotlib

## 8. Installation

Run the following command in Google Colab:

```python
!pip install pandas numpy matplotlib
```

## 9. Importing Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

## 10. Creating the Dataset

```python
data = {
    "Name": [
        "Rahul Sharma",
        "Priya Singh",
        "Aman Kumar",
        "Neha Gupta",
        "Rohit Verma",
        "Anjali Sharma"
    ],
    "Email": [
        "rahul@gmail.com",
        "priya@gmail.com",
        "aman@gmail.com",
        "neha@gmail.com",
        "rohit@gmail.com",
        "anjali@gmail.com"
    ],
    "Phone": [
        "9876543210",
        "9876543211",
        "9876543212",
        "9876543213",
        "9876543214",
        "9876543215"
    ],
    "Age": [21, 22, 21, 22, 23, 21],
    "Gender": [
        "Male", "Female", "Male",
        "Female", "Male", "Female"
    ],
    "City": [
        "Delhi", "Delhi", "Noida",
        "Noida", "Ghaziabad", "Delhi"
    ],
    "Course": ["BCA", "BCA", "BCA", "BCA", "BCA", "BCA"],
    "Marks": [85, 91, 78, 88, 72, 95]
}

df = pd.DataFrame(data)

print("Original Dataset:")
display(df)
```

## 11. Inspecting the Dataset

The dataset is inspected to understand its size, structure, and statistical information.

```python
print("Dataset Shape:")
print(df.shape)

print("\nDataset Information:")
df.info()

print("\nStatistical Summary:")
display(df.describe())
```

The shape function displays the number of rows and columns, while the info function provides information about data types and missing values.

## 12. Identifying Sensitive Information

The dataset contains the following sensitive information:

**Direct Identifiers:**
- Name
- Email
- Phone

**Quasi-Identifiers:**
- Age
- Gender
- City
- Course

**Sensitive Attribute:**
- Marks

These attributes are identified before applying anonymization techniques.

## 13. Technique 1 – Data Masking

### Definition

Data masking is a technique in which sensitive information is partially hidden or replaced with other characters to prevent unauthorized viewing.

For example:

Original Phone Number: 9876543210

Masked Phone Number: XXXXXX3210

Data masking is commonly used when displaying sensitive information in applications and reports.

### 13.1 Masking Phone Numbers

```python
df_masked = df.copy()

df_masked["Phone"] = df_masked["Phone"].apply(
    lambda x: "X" * (len(x) - 4) + x[-4:]
)

print("Dataset After Phone Masking:")
display(df_masked)
```

**Output Example:**

| Name | Phone |
|---|---|
| Rahul Sharma | XXXXXX3210 |
| Priya Singh | XXXXXX3211 |
| Aman Kumar | XXXXXX3212 |

The first six digits are hidden, while the last four digits remain visible.

### 13.2 Masking Email Addresses

```python
def mask_email(email):
    username, domain = email.split("@")

    if len(username) <= 2:
        masked_username = "X" * len(username)
    else:
        masked_username = (
            username[0] +
            "X" * (len(username) - 2) +
            username[-1]
        )

    return masked_username + "@" + domain

df_masked["Email"] = df_masked["Email"].apply(mask_email)

print("Dataset After Email Masking:")
display(df_masked)
```

**Example:**

Original: rahul@gmail.com

Masked: rXXXl@gmail.com

The username is partially hidden while the email domain remains visible.

## 14. Removing Direct Identifiers

Direct identifiers can also be removed completely from the dataset.

```python
df_removed = df.copy()

df_removed = df_removed.drop(
    columns=["Name", "Email", "Phone"]
)

print("Dataset After Removing Direct Identifiers:")
display(df_removed)
```

This removes information that directly identifies individuals while retaining other attributes for analysis.

Note: Removing direct identifiers alone does not guarantee complete anonymization because quasi-identifiers may still reveal identities.

## 15. Technique 2 – k-Anonymity

### Definition

k-Anonymity is a privacy technique in which each combination of quasi-identifiers must appear in at least k records.

For example, if k = 2, every combination of selected quasi-identifiers should be shared by at least two records.

**Formula:**

|Equivalence Class| ≥ k

An equivalence class is a group of records having identical quasi-identifier values.

### 15.1 Generalization

Generalization means replacing specific information with broader categories.

For example:

Original Ages:

21, 22, 23

Generalized Ages:

18–25, 18–25, 18–25

This makes individual records less distinguishable.

### 15.2 Age Generalization

```python
df_k = df.copy()

def generalize_age(age):
    if 18 <= age <= 25:
        return "18-25"
    elif 26 <= age <= 35:
        return "26-35"
    else:
        return "36+"

df_k["Age"] = df_k["Age"].apply(generalize_age)

print("After Age Generalization:")
display(df_k)
```

All students between 18 and 25 years are represented in the same age group.

### 15.3 City Generalization

Cities can be generalized into broader geographical regions.

```python
city_mapping = {
    "Delhi": "Delhi-NCR",
    "Noida": "Delhi-NCR",
    "Ghaziabad": "Delhi-NCR"
}

df_k["City"] = df_k["City"].replace(city_mapping)

print("After City Generalization:")
display(df_k)
```

Now Delhi, Noida, and Ghaziabad are represented as Delhi-NCR.

### 15.4 Checking k-Anonymity

We group records using quasi-identifiers and count the number of records in each group.

```python
quasi_identifiers = [
    "Age",
    "Gender",
    "City",
    "Course"
]

equivalence_classes = (
    df_k.groupby(quasi_identifiers)
    .size()
    .reset_index(name="Count")
)

print("Equivalence Classes:")
display(equivalence_classes)
```

### 15.5 Checking k = 2

```python
k = 2

equivalence_classes["Meets_k"] = (
    equivalence_classes["Count"] >= k
)

print("k-Anonymity Verification:")
display(equivalence_classes)
```

The dataset satisfies 2-anonymity when every equivalence class contains at least two records.

In this example, the generalized records form two groups, each containing three students.

## 16. Technique 3 – Differential Privacy

### Definition

Differential Privacy is a mathematical technique used to protect an individual's information while allowing statistical analysis of a dataset.

It works by adding controlled random noise to the results of queries.

For example:

Actual Average Marks = 84.83

Privacy-Protected Average Marks = 85.41

The released result is slightly different from the actual result, making it harder to determine an individual's contribution.

### 16.1 Laplace Mechanism

The Laplace mechanism is used to add random noise to numerical results.

**Formula:**

Noisy Result = Actual Result + Laplace Noise

The amount of noise depends on:

1. Sensitivity: How much one individual's data can affect the result.
2. Epsilon (ε): The privacy parameter that controls the amount of noise.

A smaller epsilon generally provides stronger privacy but introduces more noise.

## 17. Implementing Differential Privacy

First, calculate the actual average marks.

```python
actual_average = df["Marks"].mean()

print("Actual Average Marks:", actual_average)
```

### 17.1 Adding Laplace Noise

```python
epsilon = 1.0

# Marks are assumed to be between 0 and 100.
sensitivity = 100 / len(df)

noise = np.random.laplace(
    loc=0,
    scale=sensitivity / epsilon
)

private_average = actual_average + noise

print("Actual Average:", actual_average)
print("Noise Added:", noise)
print("Differentially Private Average:", private_average)
```

The program adds random Laplace noise to the actual average marks.

The resulting average is a privacy-protected statistical output.

## 18. Comparing Different Privacy Levels

Different epsilon values can be used to understand how privacy affects accuracy.

```python
actual_average = df["Marks"].mean()

epsilons = [0.1, 0.5, 1.0, 2.0]

sensitivity = 100 / len(df)

results = []

for epsilon in epsilons:

    noise = np.random.laplace(
        loc=0,
        scale=sensitivity / epsilon
    )

    private_average = actual_average + noise

    results.append({
        "Epsilon": epsilon,
        "Noise": noise,
        "Private Average": private_average
    })

dp_results = pd.DataFrame(results)

print("Differential Privacy Results:")
display(dp_results)
```

Smaller epsilon values generally produce more noise, while larger epsilon values generally produce less noise.

Note: This is a simplified classroom demonstration of the Laplace mechanism. The sensitivity is calculated for a bounded 0–100 marks range, assuming a fixed-size dataset and one individual's contribution is replaced.

## 19. Visualizing Differential Privacy

The effect of different epsilon values can be visualized using Matplotlib.

```python
plt.figure(figsize=(8, 5))

plt.plot(
    dp_results["Epsilon"],
    dp_results["Private Average"],
    marker="o"
)

plt.axhline(
    actual_average,
    linestyle="--",
    label="Actual Average"
)

plt.xlabel("Epsilon (ε)")
plt.ylabel("Average Marks")

plt.title("Effect of Differential Privacy")

plt.legend()
plt.grid(True)
plt.show()
```

The graph compares the actual average marks with the privacy-protected average marks obtained at different epsilon values.

## 20. Complete Anonymization Workflow

The overall anonymization process is:

Original Dataset  
↓  
Identify Sensitive Information  
↓  
Apply Anonymization Techniques  
↓  
Data Masking – Hide Sensitive Values  
↓  
k-Anonymity – Generalize Quasi-Identifiers  
↓  
Differential Privacy – Add Controlled Noise  
↓  
Generate Privacy-Protected Results  
↓  
Data Analysis and Sharing

## 21. Before and After Anonymization

### Before Anonymization

| Name | Email | Phone | Age | City |
|---|---|---|---:|---|
| Rahul Sharma | rahul@gmail.com | 9876543210 | 21 | Delhi |
| Priya Singh | priya@gmail.com | 9876543211 | 22 | Delhi |
| Aman Kumar | aman@gmail.com | 9876543212 | 21 | Noida |

The original dataset contains direct identifiers and exact personal information.

### After Data Masking

| Name | Email | Phone |
|---|---|---|
| Rahul Sharma | rXXXl@gmail.com | XXXXXX3210 |
| Priya Singh | pXXXa@gmail.com | XXXXXX3211 |
| Aman Kumar | aXXn@gmail.com | XXXXXX3212 |

Email addresses and phone numbers are partially hidden.

### After k-Anonymity

| Age | Gender | City | Course |
|---|---|---|---|
| 18–25 | Male | Delhi-NCR | BCA |
| 18–25 | Female | Delhi-NCR | BCA |
| 18–25 | Male | Delhi-NCR | BCA |

The direct identifiers have been removed and quasi-identifiers have been generalized.

### After Differential Privacy

The actual average marks are calculated and random noise is added to produce a privacy-protected statistical result.

## 22. Privacy-Utility Trade-off

Anonymization involves balancing privacy protection and data usefulness.

**More Privacy:**
- More generalization
- More information masking
- Greater noise in statistical results

**More Utility:**
- More accurate information
- Less modification of original data
- More useful analytical results

If too much information is removed or modified, the dataset becomes less useful.

If very little information is modified, individuals may remain identifiable.

Therefore, an appropriate balance between privacy and utility is necessary.

## 23. Comparison of Anonymization Techniques

| Technique | Main Purpose | Method | Example |
|---|---|---|---|
| Data Masking | Hide sensitive information | Replace characters | XXXXXX3210 |
| k-Anonymity | Reduce identification risk | Generalization | 21 → 18–25 |
| Differential Privacy | Protect individual contributions | Add mathematical noise | 84.83 → 85.41 |

## 24. Advantages

### Data Masking
- Simple to implement.
- Protects sensitive information from direct viewing.
- Useful for displaying information in applications.
- Helps reduce accidental exposure.

### k-Anonymity
- Reduces the uniqueness of individual records.
- Helps prevent re-identification.
- Useful for publishing and sharing datasets.
- Provides a measurable privacy requirement through k.

### Differential Privacy
- Provides a mathematical privacy framework.
- Protects individual contributions to statistical outputs.
- Useful for research and statistical analysis.
- Allows organizations to release useful aggregate information with privacy guarantees.

## 25. Limitations

### Data Masking
- Does not completely prevent identification.
- Some original information may still be inferred.
- Improper masking may expose sensitive data.

### k-Anonymity
- Can be vulnerable to attacks involving sensitive attributes.
- Excessive generalization can reduce data usefulness.
- Selecting an appropriate value of k is important.

### Differential Privacy
- Adding too much noise can reduce accuracy.
- Selecting epsilon requires careful consideration.
- Correct implementation requires understanding sensitivity and privacy assumptions.

## 26. Real-World Applications

Data anonymization is used in several fields:

1. **Healthcare:** Protecting patient information while sharing medical research data.
2. **Banking:** Protecting customer information during financial analysis.
3. **Education:** Protecting student records while analyzing academic performance.
4. **Government:** Publishing statistical information without exposing citizens' identities.
5. **Marketing:** Analyzing customer behavior while reducing exposure of personal information.

## 27. Result

In this practical, different data anonymization techniques were successfully studied and implemented using Python.

Data masking was used to hide sensitive information, k-anonymity was applied through generalization of quasi-identifiers, and differential privacy was demonstrated by adding Laplace noise to statistical results.

The comparison between original and anonymized data demonstrated how personal information can be protected while retaining useful information for analysis.

**Hence, the practical successfully demonstrates the implementation of data anonymization techniques for protecting personal information and maintaining data privacy.**
