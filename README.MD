# test

## Data Provided

### Table: agents
| id | name   | officeId |
|----|--------|----------|
| 1  | Zoe    | 1        |
| 2  | Hazel  | 2        |
| 3  | Bella  | 2        |
| 4  | Mary   | 2        |
| 5  | Skylar | null     |

### Table: offices
| id | address        | directorId |
|----|----------------|------------|
| 1  | New York       | 1          |
| 2  | San Francisco  | 2          |
| 3  | Chicago        | null       |

### Table: employees
| id | name       |
|----|------------|
| 1  | Employee 1 |
| 2  | Employee 2 |
| 3  | Employee 3 |

### Table: sales
| employeeId | value |
|------------|-------|
| 1          | 100   |
| 1          | 200   |
| 2          | 300   |

For each SQL Querry, specify the number of rows it should return

### Querry 1
```sql
SELECT name FROM agents JOIN offices ON agents.officeId = offices.Id;
```
**Explanation:**
- INNER JOIN returns only rows where there is a match in both tables.
- Matching rows:
  - Zoe → officeId 1 → matches offices.id = 1 (New York)
  - Hazel, Bella, Mary → officeId 2 → matches offices.id = 2 (San Francisco)
  - Skylar → officeId null → no match (excluded)
  - Offices.id = 3 → no agent has officeId = 3 (excluded).

**Result:**
| name   |
|--------|
| Zoe    |
| Hazel  |
| Bella  |
| Mary   |

**Number of rows:** 4

### Querry 2
```sql
SELECT name FROM agents LEFT JOIN offices ON agents.officeId = offices.Id;
```
**Explanation:**
- LEFT JOIN returns all rows from agents, plus matching rows from offices (or nulls if no match).
- All 5 agents from agents table:
  - Zoe, Hazel, Bella, Mary → each matches an office.
  - Skylar → officeId null → no match in offices → office columns will be null.

**Result:**
| name   |
|--------|
| Zoe    |
| Hazel  |
| Bella  |
| Mary   |
| Skylar |

**Number of rows:** 5

### Querry 3
```sql
SELECT name FROM agents RIGHT JOIN offices ON offices.directorId = agents.Id;
```
**Explanation:**
- RIGHT JOIN returns all rows from offices, plus matching rows from agents (or nulls if no match).
- Match condition: offices.directorId = agents.Id
  - Office 1: directorId = 1 → matches agent Zoe (id=1) → name = Zoe
  - Office 2: directorId = 2 → matches agent Hazel (id=2) → name = Hazel
  - Office 3: directorId = null → no matching agent → name = null

**Result:**
| name   |
|--------|
| Zoe    |
| Hazel  |
| null   |

**Number of rows:** 3



### Question 4

**Data Provided:**
#### Table: employees
| id | name       |
|----|------------|
| 1  | Employee 1 |
| 2  | Employee 2 |
| 3  | Employee 3 |

#### Table: sales
| employeeId | value |
|------------|-------|
| 1          | 100   |
| 1          | 200   |
| 2          | 300   |

**Question:**
Write a query that returns names of employees who have no sales.

```sql
SELECT e.name
FROM employees e
LEFT JOIN sales s ON e.id = s.employeeId
WHERE s.employeeId IS NULL;
```
**Explanation:**
- This query uses a LEFT JOIN to find employees who have no matching rows in the `sales` table.
- The condition `WHERE s.employeeId IS NULL` filters out employees who have sales.



### Question 5

**Data Provided:**

#### Table: employees
| id | mgrId | name       |
|----|-------|------------|
| 1  | NULL  | Employee 1 |
| 2  | 1     | Employee 2 |
| 3  | 1     | Employee 3 |

**Question:**
Write a select query that returns:
- The name of the employee.
- The name of the manager of the employee. If there is no manager, it should be NULL.

Managers are considered to be employees as well.

```sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m ON e.mgrId = m.id;
```
**Explanation:**
- This query uses a LEFT JOIN to match employees with their managers.
- If an employee does not have a manager (`mgrId IS NULL`), the manager column will return NULL.

**Result:**
- Returns the name of each employee and their respective manager (or NULL if no manager).



### Question 6
**Question:**
Which quarter had the lowest profit margin (ratio of profit to revenue)?

**Graph:**
![Graph showing revenue and costs per quarter](https://github.com/gregory-bot/data-test/blob/main/q6.jpeg)

**Explanation:**
- Profit margin is calculated as `(Revenue - Costs) / Revenue`.
- Analyze the graph:
  - Q1: Revenue = 200, Costs = 100 → Profit margin = (200 - 100) / 200 = 0.5
  - Q2: Revenue = 300, Costs = 200 → Profit margin = (300 - 200) / 300 = 0.33
  - Q3: Revenue = 300, Costs = 150 → Profit margin = (300 - 150) / 300 = 0.5
  - Q4: Revenue = 400, Costs = 300 → Profit margin = (400 - 300) / 400 = 0.25

**Answer:**
- Q4 has the lowest profit margin (0.25).

### Question 7
**Question:**
What is the average profit per quarter?

**Graph:**
![Graph showing quarterly revenues and costs](graph_question_7.png)

**Explanation:**
- Profit is calculated as `Revenue - Cost`.
- Average Profit is calculated as `(Sum of Profit for all quarters) ÷ 4`.

**Estimation from the graph:**
- Q1: Revenue ~$150,000, Cost ~$100,000 → Profit ~$50,000
- Q2: Revenue ~$125,000, Cost ~$100,000 → Profit ~$25,000
- Q3: Revenue ~$175,000, Cost ~$125,000 → Profit ~$50,000
- Q4: Revenue ~$150,000, Cost ~$100,000 → Profit ~$50,000

**Calculations:**
- Total Profit = $50,000 + $25,000 + $50,000 + $50,000 = $175,000
- Average Profit = $175,000 ÷ 4 = $43,750

**Answer:**
- $43,750

### Question 8
**Question:**
Which members of the team achieved above average results for the first quarter of the year?

**Graph:**
![Graph showing KPI metrics for team members](graph_question_8.png)

**Explanation:**
- Calculate the average number of closed deals for Q1 across all team members.
- Sum of closed deals:
  - Anthony: January = 20, February = 25, March = 30 → Total = 75
  - Charlotte: January = 15, February = 20, March = 25 → Total = 60
  - Elizabeth: January = 10, February = 15, March = 20 → Total = 45
  - Eloise: January = 30, February = 35, March = 40 → Total = 105
  - Liam: January = 25, February = 30, March = 35 → Total = 90
- Total deals = 75 + 60 + 45 + 105 + 90 = 375
- Average deals = 375 ÷ 5 = 75

**Above average performers:**
- Eloise (105 deals)
- Liam (90 deals)

### Question 9
**Question:**
After returning to the US from vacation, you check the currency exchange rates to see how much the leftover foreign cash you have is worth.

**Exchange Rates:**
| Currency | 1 USD = |
|----------|---------|
| USD      | 1       |
| EUR      | 2       |
| GBP      | 5       |
| SEK      | 0.5     |

**Leftover Foreign Cash:**
- 50 EUR
- 10 GBP
- 100 SEK

**Question:**
How much is the leftover foreign cash worth in US dollars (USD)?

**Calculations:**
- 50 EUR → 50 ÷ 2 = 25 USD
- 10 GBP → 10 ÷ 5 = 2 USD
- 100 SEK → 100 ÷ 0.5 = 50 USD

**Total:**
- 25 + 2 + 50 = 77 USD

**Answer:**
- 77 USD

### Question 10
**Question:**
The price of gas was $4.00 per gallon. It increased by 20% and then decreased by 20%. What is the gas price now?

**Calculations:**
1. Initial price = $4.00
2. After a 20% increase:
   - New price = $4.00 × (1 + 0.20) = $4.80
3. After a 20% decrease:
   - New price = $4.80 × (1 - 0.20) = $3.84

**Answer:**
- $3.84 per gallon

### Question 11
**Question:**
Last year, a bakery owner occasionally counted the number of coffees and cakes he sold for the day. He created the following chart from his compiled numbers.

**Graph:**
![Graph showing relationship between coffee and cake sales](graph_question_11.png)

**Question:**
Given his data, which of the following conclusions are true? (Select all acceptable answers.)

**Analysis:**
- The graph shows an inverse relationship between coffee sales and cake sales.
- As the number of coffees sold increases, the number of cakes sold decreases.

**Conclusions:**
1. Higher coffee sales decrease cake sales. (True)
2. Higher cake sales decrease coffee sales. (True)
3. When more cakes than average are sold, fewer coffees are sold. (True)
4. When more coffees than average are sold, fewer cakes are sold. (True)
5. Coffee sales and cake sales will never increase at the same time. (True)
