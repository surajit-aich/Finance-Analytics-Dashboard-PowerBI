# DAX Measures — Finance Analytics Dashboard

All DAX measures used in this Power BI project are listed below, grouped by category.

---

## 1. Base Measures

**Total Amount**
```dax
Total Amount = SUM(fact_finance_transactions[amount])
```

**Total Transactions**
```dax
Total Transactions = DISTINCTCOUNT(fact_finance_transactions[transaction_id])
```

**Total Fees**
```dax
Total Fees = SUM(fact_finance_transactions[fee_amount])
```

**Total Tax**
```dax
Total Tax = SUM(fact_finance_transactions[tax_amount])
```

**Average Transaction Value**
```dax
Average Transaction Value = AVERAGE(fact_finance_transactions[amount])
```

---

## 2. Previous Year Measures

**Previous Year Total Amount**
```dax
Previous Year Total Amount = 
CALCULATE([Total Amount],
SAMEPERIODLASTYEAR('dim_calendar(date)'[Date]))
```

**PY Total Transactions**
```dax
PY Total Transactions = 
CALCULATE([Total Transactions],
SAMEPERIODLASTYEAR('dim_calendar(date)'[Date]))
```

**PY Total Fees**
```dax
PY Total Fees = 
CALCULATE([Total Fees],
SAMEPERIODLASTYEAR('dim_calendar(date)'[Date]))
```

**PY Total Tax**
```dax
PY Total Tax = 
CALCULATE([Total Tax],
SAMEPERIODLASTYEAR('dim_calendar(date)'[Date]))
```

**PY Average Transaction Value**
```dax
PY Average Transaction Value = 
CALCULATE([Average Transaction Value],
SAMEPERIODLASTYEAR('dim_calendar(date)'[Date]))
```

---

## 3. YoY Growth %

**Growth %**
```dax
Growth % = 
VAR Current_value = [Total Amount]
VAR Previous_value = [Previous Year Total Amount]
RETURN
IF(ISBLANK(Previous_value),
BLANK(),
DIVIDE(Current_value-Previous_value,Previous_value))
```

**YOY Transactions Growth %**
```dax
YOY Transactions Growth % = 
VAR Total_transactions = [Total Transactions]
VAR PY_Total_transactions = [PY Total Transactions]
RETURN
IF(ISBLANK(PY_Total_transactions),
BLANK(),
DIVIDE(Total_transactions-PY_Total_transactions,PY_Total_transactions))
```

**YoY Fees Growth %**
```dax
YoY Fees Growth % = 
VAR Total_fees = [Total Fees]
VAR PYTotal_fees = [PY Total Fees]
RETURN
IF(ISBLANK(PYTotal_fees),
BLANK(),
DIVIDE(Total_fees-PYTotal_fees,PYTotal_fees))
```

**YoY Tax Growth %**
```dax
YoY Tax Growth % = 
VAR Total_tax = [Total Tax]
VAR PYTotal_tax = [PY Total Tax]
RETURN
IF(ISBLANK(PYTotal_tax),
BLANK(),
DIVIDE(Total_tax-PYTotal_tax,PYTotal_tax))
```

**YoY Average Transaction Growth %**
```dax
YoY Average Transaction Growth % = 
VAR Avg_trans_Value = [Average Transaction Value]
VAR PYAvg_trans_value = [PY Average Transaction Value]
RETURN
IF(ISBLANK(PYAvg_trans_value),
BLANK(),
DIVIDE(Avg_trans_Value-PYAvg_trans_value,PYAvg_trans_value))
```

---

## 4. Absolute Change

**Absolute Change**
```dax
Absolute Change = [Total Amount]-[Previous Year Total Amount]
```

**Absolute Change Transactions**
```dax
Absolute Change Transactions = [Total Transactions]-[PY Total Transactions]
```

---

## 5. Display Measures

**Growth Display KPI**
```dax
Growth Display KPI = 
VAR Growth = [Growth %]
RETURN
SWITCH(TRUE(),
ISBLANK(Growth),BLANK(),
Growth>= 0,UNICHAR(9650)&" "&FORMAT(Growth,"0.00%"),
UNICHAR(9660)&" "&FORMAT(Growth,"0.00%"))
```

**YOY Transaction Growth Display**
```dax
YOY Transaction Growth Display = 
VAR Growth = [YOY Transactions Growth %]
RETURN
SWITCH(TRUE(),
ISBLANK(Growth),BLANK(),
Growth>= 0,UNICHAR(9650)&" "&FORMAT(Growth,"0.0%"),
UNICHAR(9660)&" "&FORMAT(Growth,"0.0%"))
```

**Fees Growth Display**
```dax
Fees Growth Display = 
VAR Change = [YoY Fees Growth %]
RETURN
SWITCH(TRUE(),
    ISBLANK(Change), BLANK(),
    Change >= 0, UNICHAR(9650) & " " & FORMAT(Change, "0.0%"),
    UNICHAR(9660) & " " & FORMAT(Change, "0.0%"))
```

**Tax Growth Display**
```dax
Tax Growth Display = 
VAR Change = [YoY Tax Growth %]
RETURN
SWITCH(TRUE(),
    ISBLANK(Change), BLANK(),
    Change >= 0, UNICHAR(9650) & " " & FORMAT(Change, "0.0%"),
    UNICHAR(9660) & " " & FORMAT(Change, "0.0%"))
```

**Average Amount Growth Display**
```dax
Average Amount Growth Display = 
VAR Change = [YoY Average Transaction Growth %]
RETURN
SWITCH(TRUE(),
    ISBLANK(Change), BLANK(),
    Change >= 0, UNICHAR(9650) & " " & FORMAT(Change, "0.0%"),
    UNICHAR(9660) & " " & FORMAT(Change, "0.0%"))
```

**Absolute Change Display**
```dax
Absolute Change Display = 
VAR Change = [Absolute Change]
RETURN
SWITCH(TRUE(),
    ISBLANK(Change), BLANK(),
    Change >= 0, UNICHAR(9650) & " ₹" & FORMAT(Change/1000000, "0.0") & "M",
    UNICHAR(9660) & " ₹" & FORMAT(ABS(Change)/1000000, "0.0") & "M")
```

**Absolute Change Display Transactions**
```dax
Absolute Change Display Transactions = 
VAR Change = [Absolute Change Transactions]
RETURN
SWITCH(TRUE(),
    ISBLANK(Change), BLANK(),
    Change >= 0, UNICHAR(9650) & " " & FORMAT(ABS(Change)/1000, "0.0") & "K Txns",
    UNICHAR(9660) & " " & FORMAT(ABS(Change)/1000, "0.0") & "K Txns")
```

---

## 6. Colour Measures

**Growth Colour KPI**
```dax
Growth Colour KPI = 
VAR Growth = [Growth %]
RETURN
SWITCH(TRUE(),
ISBLANK(Growth),"#808080",
Growth>= 0,"#2E7D32",
"#D32F2F")
```

**Transactions Growth Colour**
```dax
Transactions Growth Colour = 
VAR Growth = [YOY Transactions Growth %]
RETURN
SWITCH(TRUE(),
ISBLANK(Growth),"#808080",
Growth>= 0,"#2E7D32",
"#D32F2F")
```

**Fees Growth Colour**
```dax
Fees Growth Colour = 
VAR Growth = [YoY Fees Growth %]
RETURN
SWITCH(TRUE(),
ISBLANK(Growth),"#808080",
Growth>= 0,"#2E7D32",
"#D32F2F")
```

**Tax Growth Colour**
```dax
Tax Growth Colour = 
VAR Growth = [YoY Tax Growth %]
RETURN
SWITCH(TRUE(),
ISBLANK(Growth),"#808080",
Growth>= 0,"#2E7D32",
"#D32F2F")
```

**Average Amount Growth Colour**
```dax
Average Amount Growth Colour = 
VAR Growth = [YoY Average Transaction Growth %]
RETURN
SWITCH(TRUE(),
ISBLANK(Growth),"#808080",
Growth>= 0,"#2E7D32",
"#D32F2F")
```

**Absolute Change Colour**
```dax
Absolute Change Colour = 
VAR Change = [Absolute Change]
RETURN
SWITCH(TRUE(),
    ISBLANK(Change), "#808080",
    Change >= 0, "#2E7D32",
    "#D32F2F")
```

**Absolute Change Colour Transactions**
```dax
Absolute Change Colour Transactions = 
VAR Change = [Absolute Change Transactions]
RETURN
SWITCH(TRUE(),
    ISBLANK(Change), "#808080",
    Change >= 0, "#2E7D32",
    "#D32F2F")
```

---

## 7. Dynamic Measure

**Dynamic Measure** *(Field Parameter)*
```dax
Dynamic Measure = {
    ("Total Amount", NAMEOF('_Measures'[Total Amount]), 0),
    ("Total Transactions", NAMEOF('_Measures'[Total Transactions]), 1),
    ("Total Fees", NAMEOF('_Measures'[Total Fees]), 2),
    ("Total Tax", NAMEOF('_Measures'[Total Tax]), 3)
}
```

**Dynamic Title**
```dax
Dynamic Title = 
SWITCH('Dynamic Measure'[Dynamic Measure Order],
    0, "Total Amount",
    1, "Total Transactions",
    2, "Total Fees",
    3, "Total Tax",
    "Other")
```

**Dynamic Rank**
```dax
Dynamic Rank = 
RANKX(
    ALL(dim_customers[state]),
    SWITCH(
        SELECTEDVALUE('Dynamic Measure'[Dynamic Measure Order]),
        0, [Total Amount],
        1, [Total Transactions],
        2, [Total Fees],
        3, [Total Tax]
    )
)
```

---

## 8. Chart Titles

**Area Chart Title**
```dax
Area Chart Title = 
SELECTEDVALUE('Dynamic Measure'[Dynamic Title]) & " by Month"
```

**Bar Chart Title**
```dax
Bar Chart Title = 
SELECTEDVALUE('Dynamic Measure'[Dynamic Title]) & " by Customer Segment"
```

**Channel Chart Title**
```dax
Channel Chart Title = 
SELECTEDVALUE('Dynamic Measure'[Dynamic Title]) & " by Channel"
```

**Donut Chart Title**
```dax
Donut Chart Title = 
SELECTEDVALUE('Dynamic Measure'[Dynamic Title]) & " by Transaction Status"
```

**State Bar Chart Title**
```dax
State Bar Chart Title = 
SELECTEDVALUE('Dynamic Measure'[Dynamic Title]) & " by State"
```

---

## 9. Calendar Columns

**dim_calendar(date)**
```dax
dim_calendar(date) = CALENDAR(MIN(fact_finance_transactions[transaction_date]),
    MAX(fact_finance_transactions[transaction_date]))
```

**Month Name**
```dax
Month Name = FORMAT('dim_calendar(date)'[Date],"MMM")
```

**Month No**
```dax
Month No = MONTH('dim_calendar(date)'[Date])
```

**Quarter**
```dax
Quarter = "Q-" & QUARTER('dim_calendar(date)'[Date])
```

**Year**
```dax
Year = YEAR('dim_calendar(date)'[Date])
```
