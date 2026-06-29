Source: #source/internet_resources 
Project: #project/devops 
Areas: #area/work
Subject: #subject/automation
Type: #type/article 
Learning priority: 
Status: #status/finished
Related: [[AI]]
[[Longlife learning]]
[[Python]]
[[GOLANG]]

---
# Automation in Infrastructure

Many infrastructure-related tasks are repetitive, time-consuming, and often need to be performed on a large scale. Automating these processes increases efficiency, reduces human error, and allows teams to focus on more strategic activities.

Examples of tasks that can benefit from automation include:

* Changing IP addresses on multiple servers
* Copying files across many machines
* Updating operating systems and applications
* Automatically recovering failed machines or services
* Managing backups and snapshots
* Collecting inventory and system information
* Performing compliance and security checks
* Any other repetitive operational task

To automate these activities, we can use programming and scripting languages such as:

* [[Shell Scripting]]
* [[Python]]
* [[Go]]
* Other programming languages

Modern [[AI]] tools can significantly accelerate the development process. Instead of writing everything from scratch, you can ask an AI assistant to generate an entire script or individual code blocks, then refine and combine them as needed.

For example, you can use the prompt below to generate a Python script that reads a CSV file, calculates the sum of the values in each row, and displays the result.

```text
You are a Python developer.

Create a Python script that reads a CSV file named `source.csv`. For each row, sum all comma-separated numeric values and print the row number together with the calculated total.

The goal is to automate this calculation process and improve productivity.

Provide:

1. A complete Python script.
2. A `README.md` file with instructions on how to run and use the script.

Do not include comments in the code.
```

---

## Example AI Output

### Python Script (`sum_csv_rows.py`)

```python
import csv

with open("source.csv", newline="", encoding="utf-8") as file:
    reader = csv.reader(file)

    for row_number, row in enumerate(reader, start=1):
        total = sum(float(value.strip()) for value in row if value.strip())
        print(f"Row {row_number}: {total}")
```

### README.md

````markdown
# CSV Row Sum Calculator

This script reads a CSV file named `source.csv`, calculates the sum of all numeric values in each row, and prints the row number together with the calculated total.

## Requirements

- Python 3.6 or later

## Project Structure

```text
.
├── source.csv
├── sum_csv_rows.py
└── README.md
```

## Example CSV File

```csv
10,20,30
5,15,25
100,200,300
```

## Running the Script

Execute the following command:

```bash
python sum_csv_rows.py
```

## Example Output

```text
Row 1: 60.0
Row 2: 45.0
Row 3: 600.0
```

## Notes

- All values must be numeric.
- Empty values are ignored.
- The script reads `source.csv` from the current directory.
````

