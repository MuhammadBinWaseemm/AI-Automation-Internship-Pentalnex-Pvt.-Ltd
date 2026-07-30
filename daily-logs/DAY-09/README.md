# Day 09 – JavaScript for Automation (Code Node)

## Project Name
Messy Dataset Transformation using n8n Code Node

---

## Problem Statement

Organizations often receive data with inconsistent formatting, such as names with extra spaces, mixed uppercase/lowercase letters, and emails with inconsistent casing. This project cleans and standardizes the data while assigning grades based on candidate scores.

---

## Objective

- Normalize names by removing extra spaces and converting them to Title Case.
- Convert email addresses to lowercase.
- Calculate a grade for each record based on the score.
- Filter the dataset to keep only records with Grade **A** or **B**.
- Perform all transformations using only the **Code** node in n8n.

---

## Workflow Architecture

```
Manual Trigger
       │
       ▼
   Code Node
       │
       ▼
 Transformed Output
```

---

## Technologies Used

- n8n
- JavaScript (ES6)
- Code Node
- JSON

---

## Nodes Used

### 1. Manual Trigger
Starts the workflow manually for testing.

### 2. Code Node
- Creates the sample dataset.
- Normalizes names.
- Converts emails to lowercase.
- Calculates grades.
- Filters records to keep only Grade A and Grade B.
- Returns the transformed JSON data.

---

## Setup Instructions

1. Open n8n.
2. Create a new workflow.
3. Add a **Manual Trigger** node.
4. Add a **Code** node.
5. Connect the Manual Trigger to the Code node.
6. Paste the provided JavaScript code into the Code node.
7. Execute the workflow.
8. View the transformed records in the Output panel.

---

## Credentials Required

None

---

## Workflow Explanation

The workflow begins with a Manual Trigger.

The Code node contains a dataset of 15 records with inconsistent formatting. It first normalizes each name by trimming extra spaces and converting it to Title Case. It then converts every email address to lowercase.

Next, a grade is assigned using the following criteria:

- Score ≥ 90 → Grade A
- Score 80–89 → Grade B
- Score 70–79 → Grade C
- Score 60–69 → Grade D
- Score < 60 → Grade F

Finally, the workflow filters the records so that only candidates with Grade A or Grade B are returned.

---

## JavaScript Code

```javascript
const records = [
  { name: "  ali KHAN ", email: "Ali.KHAN@GMAIL.COM", score: 92 },
  { name: "sara ahmed", email: "SARA@OUTLOOK.COM", score: 85 },
  { name: "  HAMZA ALI", email: "Hamza@Yahoo.COM", score: 78 },
  { name: "fatima noor ", email: "FATIMA@GMAIL.COM", score: 67 },
  { name: "  usman RAZA ", email: "USMAN@HOTMAIL.COM", score: 58 },
  { name: "ayesha iqbal", email: "AYESHA@GMAIL.COM", score: 88 },
  { name: "  bilal ", email: "BILAL@GMAIL.COM", score: 73 },
  { name: "zain ABBAS", email: "ZAIN@YAHOO.COM", score: 81 },
  { name: "  maryam ali ", email: "MARYAM@GMAIL.COM", score: 95 },
  { name: "hassan khan", email: "HASSAN@OUTLOOK.COM", score: 61 },
  { name: "  hiba noor", email: "HIBA@GMAIL.COM", score: 49 },
  { name: "muhammad saad", email: "SAAD@YAHOO.COM", score: 90 },
  { name: "amna", email: "AMNA@HOTMAIL.COM", score: 76 },
  { name: "  daniyal ", email: "DANIYAL@GMAIL.COM", score: 82 },
  { name: "iqra", email: "IQRA@GMAIL.COM", score: 69 }
];

function titleCase(str) {
  return str
    .trim()
    .toLowerCase()
    .split(" ")
    .filter(word => word !== "")
    .map(word => word.charAt(0).toUpperCase() + word.slice(1))
    .join(" ");
}

function getGrade(score) {
  if (score >= 90) return "A";
  if (score >= 80) return "B";
  if (score >= 70) return "C";
  if (score >= 60) return "D";
  return "F";
}

const result = records
  .map(record => ({
    Name: titleCase(record.name),
    Email: record.email.toLowerCase(),
    Score: record.score,
    Grade: getGrade(record.score)
  }))
  .filter(record => record.Grade === "A" || record.Grade === "B")
  .map(record => ({ json: record }));

return result;
```

---

## Test Cases

| Input | Expected Output |
|--------|-----------------|
| `"  ali KHAN "` | `"Ali Khan"` |
| `"Ali.KHAN@GMAIL.COM"` | `"ali.khan@gmail.com"` |
| Score = 92 | Grade A |
| Score = 82 | Grade B |
| Score = 76 | Filtered Out |
| Score = 58 | Filtered Out |

---

## Error Handling

- Removes unnecessary leading and trailing spaces.
- Handles inconsistent uppercase and lowercase characters.
- Filters out candidates whose grade is below B.
- Returns only clean and standardized JSON records.

---

## Known Limitations

- Uses a hardcoded dataset.
- Does not read data from external sources.
- Does not write results to a database or Google Sheets.
- Grade boundaries are fixed in the script.

---

## Future Improvements

- Read candidate records from Google Sheets or CSV files.
- Store processed data back into a database or spreadsheet.
- Send automated emails to shortlisted candidates.
- Add validation for missing or invalid email addresses.
- Integrate the workflow with an external HR or recruitment API.
