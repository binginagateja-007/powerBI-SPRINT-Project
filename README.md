# 📊 Power BI DAX Business Simulation — Sprint 2

A simulated real-world Power BI project where business requirements (framed as Jira tickets) were translated into production-style DAX measures for an **Executive Sales Dashboard**.

The focus wasn't writing isolated DAX functions — it was interpreting business needs, reasoning through filter context, and building measures that behave correctly under real report interactions (slicers, cross-filtering, drill-downs).

---

## 🎯 Business Scenario

A company needed an Executive Sales Dashboard serving three audiences:

| Stakeholder | Needs |
|---|---|
| **Sales Managers** | Revenue trends, YoY growth |
| **Regional Directors** | Customer contribution & ranking |
| **Product Heads** | Top/bottom product performance, revenue contribution |

Each requirement was treated as a ticket with defined acceptance criteria — measures had to stay dynamic and respond correctly to whatever filters were applied on the report canvas.

---

## 🧠 Skills Practiced

- Time Intelligence
- Filter Context & Context Transition
- Dynamic Ranking
- KPI Development
- Revenue Contribution Analysis
- Running Totals
- Customer & Product Analytics

---

## ⚙️ DAX Concepts Covered

`CALCULATE()` · `FILTER()` · `DIVIDE()` · `VALUES()` · `DISTINCTCOUNT()` · `ALL()` · `ALLSELECTED()` · `RANKX()` · `SAMEPERIODLASTYEAR()` · Variables (`VAR`/`RETURN`)

---

## 📈 KPIs & Sample Measures

### Sales Performance

```DAX
Total Revenue =
SUM ( Sales[Revenue] )

Previous Year Revenue =
CALCULATE (
    [Total Revenue],
    SAMEPERIODLASTYEAR ( 'Date'[Date] )
)

YoY Growth % =
DIVIDE (
    [Total Revenue] - [Previous Year Revenue],
    [Previous Year Revenue]
)
```

### Business KPIs

```DAX
Profit Margin % =
DIVIDE ( [Total Profit], [Total Revenue] )

Revenue Contribution % =
DIVIDE (
    [Total Revenue],
    CALCULATE ( [Total Revenue], ALL ( Product ) )
)

Avg Revenue per Customer =
DIVIDE ( [Total Revenue], DISTINCTCOUNT ( Sales[CustomerID] ) )
```

### Customer Analytics

```DAX
Customer Rank =
RANKX (
    ALLSELECTED ( Customer[CustomerName] ),
    [Total Revenue],
    ,
    DESC
)

Top 5 Customers Flag =
IF ( [Customer Rank] <= 5, "Top 5", "Other" )
```

### Product Analytics

```DAX
Product Rank =
RANKX (
    ALLSELECTED ( Product[ProductName] ),
    [Total Revenue],
    ,
    DESC
)

Bottom 10 Products Flag =
IF (
    [Product Rank] > MAXX ( ALLSELECTED ( Product[ProductName] ), [Product Rank] ) - 10,
    "Bottom 10",
    "Other"
)

Running Revenue =
CALCULATE (
    [Total Revenue],
    FILTER (
        ALLSELECTED ( 'Date'[Date] ),
        'Date'[Date] <= MAX ( 'Date'[Date] )
    )
)
```

---

## 🛠️ Tools Used

- Power BI Desktop
- DAX
- Data Modeling

---

## 🚀 Learning Outcome

This sprint strengthened my ability to:

- Translate ambiguous business requirements into precise DAX logic
- Reason about filter context instead of guessing at syntax
- Debug measures that "look right" but break under different filter states
- Build reusable, production-style KPI measures for executive reporting

---

## 👤 Author

**Bingi Nagateja**
Aspiring Data Analyst | SQL · Python · Excel · Power BI · DAX
