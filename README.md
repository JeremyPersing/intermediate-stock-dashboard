# Stock Dashboard for Intermediates and Beginners

Data Source: <a href="https://revesta.net" target="__blank">revesta.net</a>

<a href="https://www.youtube.com/watch?v=XZCLRacNBAw" target="__blank"><img src="https://github.com/JeremyPersing/intermediate-stock-dashboard/blob/main/Power%20Bi%20stock%20market%20Dashboard.png" alt="Thumbnail" /></a>

## Learn Power BI and DAX Using a Real World Data Set

## DAX Used in this Project

```
// Calculated Columns
----------------------------------------------------------------------------------------------
address = profile[city] & ", " & profile[state] & ", " & profile[country]
----------------------------------------------------------------------------------------------
Ticker & Company Name = profile[ticker] & " - " & profile[company]
----------------------------------------------------------------------------------------------

// Measures
----------------------------------------------------------------------------------------------
` = ""
----------------------------------------------------------------------------------------------
Down Arrow = UNICHAR(9660)
----------------------------------------------------------------------------------------------
Up Arrow = UNICHAR(9650)
----------------------------------------------------------------------------------------------
Total Return = 
var maxDate =  MAX(revesta_price_history[date])
var minDate =  MIN(revesta_price_history[date])

var minPrice = CALCULATE(SELECTEDVALUE(revesta_price_history[price]), revesta_price_history[date] == minDate)
var maxPrice = CALCULATE(SELECTEDVALUE(revesta_price_history[price]), revesta_price_history[date] == maxDate)
var priceDifference = maxPrice - minPrice
var yearDifference = DATEDIFF(minDate, maxDate, YEAR)
var percentageChange = DIVIDE((maxPrice - minPrice), minPrice)

var exponent = DIVIDE(1, yearDifference)
var cagr = POWER(DIVIDE(maxPrice, minPrice), exponent) - 1
var symbol = IF(cagr > 0, [Up Arrow], [Down Arrow])

return FORMAT(priceDifference, "Currency") &  "  -  " & symbol & FORMAT(percentageChange, "#,##0.##%") &  "  -  ("  &  FORMAT(cagr, "Percent") & " CAGR)"
----------------------------------------------------------------------------------------------
Total Return Color = IF (CONTAINSSTRING([Total Return], [Up Arrow]), "#0b856f", "#b5411e")
----------------------------------------------------------------------------------------------
Current Employees = 
var currTicker = SELECTEDVALUE(profile[ticker])
var maxYear = CALCULATE(MAX(revesta_stock_financials[year]), ALL(revesta_stock_financials), revesta_stock_financials[ticker] == currTicker)
var currEmployees = CALCULATE(
    MAX(revesta_stock_financials[employees]),
    revesta_stock_financials[year] == maxYear
)

return currEmployees
----------------------------------------------------------------------------------------------
Current Employees Formatted = FORMAT([Current Employees], "#,###,###") & " Current Employees"
----------------------------------------------------------------------------------------------
Employees Added = 
var currTicker = SELECTEDVALUE(profile[ticker])
var maxYear = CALCULATE(MAX(revesta_stock_financials[year]), ALL(revesta_stock_financials), revesta_stock_financials[ticker] == currTicker)
var secondMaxYear = CALCULATE(MAX(revesta_stock_financials[year]), ALL(revesta_stock_financials), revesta_stock_financials[ticker] == currTicker, revesta_stock_financials[year] <> maxYear)
var prevEmployees = CALCULATE(
    MAX(revesta_stock_financials[employees]),
    revesta_stock_financials[year] == secondMaxYear
)

return [Current Employees] - prevEmployees
----------------------------------------------------------------------------------------------
Sales / Employee = 
var currTicker = SELECTEDVALUE(profile[ticker])
var currRevenue = CALCULATE(MAX(revesta_stock_financials[revenue]), ALL(revesta_stock_financials), revesta_stock_financials[ticker] == currTicker)

return DIVIDE(currRevenue, [Current Employees])
----------------------------------------------------------------------------------------------
```
