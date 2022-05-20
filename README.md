# Green-Energy Stock Analysis
## Project Overview 
### <ins>Background</ins> 
By utilizing VBA (Microsoft Visual Basics for Applications), I was able to analyze data provided (from 2017 and 2018) specifically the ‘yearly return’ and ‘total daily volume’.

For this purpose, I used for loops, conditional formatting and statements amongst several other operators. Furthermore, on running the code and observing the run time - I refactored it such that the run-time was reduced to increase efficiency. 

I enjoyed coding in VBA immensely, owing to the ease in transfer of basic to advanced repetitive tasks. I was and am able to come back to the original or updated file in order to improve it as per my convenience at any point in time. Since, it is a standalone application - I have not had to further install any libraries / packages or applications to either update current code or run further data analysis, and it has proceeded to integrate effectively with Microsoft applications.

For your reference, within the [VBA_Challenge file](VBA_Challenge.xlsm) you can find the base code (Module 2) and the refactored code (Module 3) upon opening MIcrosoft Visual Basic Editor (Option + fn + F11 [mac users])

### <ins>Purpose</ins>
The final outcome of this data analysis pertains to Steve determining whether certain green-energy stocks are viable investments for his parents.

Quite specifically, they are looking to invest in DAQO stocks (Ticker: DQ).
According to Bloomberg: “Daqo New Energy Corporation manufactures polysilicon. The Company markets its polysilicon to photovoltaic product manufacturers who process it into ingots, wafers, cells and modules for solar power products.” (https://www.bloomberg.com/quote/DQ:US)

## Building the code and Table Outputs:
Both scripts built in Module 2 (base code) and module 3 (refactored code) presented with the same output, derriving input data fron the following  [dataset](VBA_Challenge.xlsm) under datasheets 2017 and 2018. Output was returned within a seprate worksheet titled **"All_Stock_Analysis"** presenting a table with three individual columns, each column referencing a different statistic (Ticker name, Total Daily Volume, and Yearly return) for the unique ticker that they relate to. The only diffenerce, that we shall delve into the specifics of a little later down this README, being the overall efficiency in run time of the executon of each block of code.

### <ins>Ticker</ins>:
Column A holds our ticker array `Dim tickers(12) As String` holding 12 elements - one element for each of the twelve tickers being examined. Creating a  ariable called `tickerIndex` allows us to recall and access array indexes as well as returning the specified values in their allocated location under the **"All_Stock_Analysis"** worksheet. 

###  <ins>Total Daily Volume</ins>:
In order to code the output for Total Daily Volume, we firstly had to specify an equation `tickerVolumes(tickerIndex) + Cells(i, 9).Value` that would allow for the calulation to be completed at least once for a single ticker before telling our script to loop it through the 11 remaining. by creating an array called tickerVolumes `Dim tickerVolumes(12) As Long`, we were consequently abke to use the previously created variable `tickerIndex` to properly store correct values for each ticker within the table.

Reulting in our final line looking like `tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 9).Value`

### <ins> Yearly Return</ins>:

In order to code for **Yearly Return** the use of conditional (If-Then) statemnets had to be employed
```
If Cells(i - 1, 1).Value <> ticker And Cells(i, 1) = ticker Then
            tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
            
End If

If Cells(i, 1).Value = ticker And Cells(i + 1, 1).Value <> ticker Then
            tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
            
End If
```
`tickerIndex` helps us initalze the start/end points of our loop referencing hte array of 12 previously created whereas `Dim tickerStartingPrices(12) As Single` and `Dim tickerEndingPrices(12) As Single` are variables created to store numerical values that will be called upon at a later time in our formulae to ouput the necessary results.

> Our first conditinal statemnet checks if the current row is the first row with the selected tickerIndex, this idicates the starting point of the loop.
> Our second conditional statemtn checks if the current row is the last row with the selected tickerIndex, this idicates the ending point of the loop.


#### For loops
The refacorisation of our VBA code heavily depended on the basic understanding of for loops. As these loops are responsible for executing the code in a repetitive manner, in out case, increasing our tickerIndex variable by 1  `tickerIndex = tickerIndex + 1` i.e. moving up to our next ticker within the array of 12 that was previously created 

By initializing arrays `tickerVolumes(tickerIndex) = 0` we are able to set the intial volume to zero, before entering the loop again. As we will further explore in the coming sections nested for loops as presented in our base code were the source of inefficiencies. 

### <ins>Formatting </ins>
In order to present our final table in an organized and visually engaging manner, Microsoft Visual Basic Editor allows for both conditional as well as Static formatting as one would normally do within excel itself. 

Examples of sytax and various forms of user available formatting can be found [over here](https://www.excelhowto.com/macros/formatting-a-range-of-cells-in-excel-vba/). For reference purposes I have included below lines of formatting that can be visible within the code I built:

````
'makes headers Bold
    Range("A3:C3").Font.FontStyle = "Bold"
    
'Adds a border
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    
'Formats cells B4/C4 -> B15/C15 in numerical format respectively
    Range("B4:B15").NumberFormat = "#,##0"
    Range("C4:C15").NumberFormat = "0.0%"

'Auto fits all contents of any cell found in Column B
    Columns("B").AutoFit
````

An example of conditional formatting:

```
dataRowStart = 4
    dataRowEnd = 15

    For i = dataRowStart To dataRowEnd
        
        If Cells(i, 3) > 0 Then
            
            Cells(i, 3).Interior.Color = vbGreen
            
        Else
        
            Cells(i, 3).Interior.Color = vbRed
            
        End If
        
    Next i
```
By applying conditionals we can set the circumsatnce in which the foramtting is applied i.e. in our example above should the cells contents be greater than 0, it is colored in green otherwise it is colored in red.

### <ins>Comparison after refacotring</ins>
Table below shows main diferences between code *before refactoring* and code *after refactoring* as well as their *run-time*. 

Code before refactoring (Module 1). |  Code after refactoring (Module 2).
:------------------------------------------:| :-------------------------------------:
Code with nested loops(click to enlarge).  | Code without nested loops (click to enlarge).	
![code before refactoring](resources/base_code.png) | ![code after refactoring](resources/refactored_code.png)
The code utlizing nested loops causes back and forth switching within worksheets eating up valuable time. | Code staying within the same loop, gathers all data and stores it in arrays where then a separate for loop the results are populated in the selected worksheet making effecient use of "batch processing' to save time.  
 Execution time of the base code: |  Execution time of the refactored code:
<img src="resources/2017_base.png" width="400" height="350"> | <img src="resources/VBA_Challenge_2017.png" width="400" height="350">

<p align="center">
:heavy_check_mark: The refactored code runs more than 5 times faster (5.33 to be precise).
</p>

## Comparison and Analysis of  green energy stocks - 2017 vs 2018
(Figure 1) | (Figure 2)
:------------------------------------------:| :-------------------------------------:	
![](resources/2017_table_analysis.png) | ![](resources/2018_table_analysis.png)
All stocks Result 2017 (click to enlarge).  | All stocks Result 2018 (click to enlarge).

### <ins>Annual Returns (%)</ins>
The table shown in Fig. 1 and Fig. 2 have columns that refer to each of the 12 company’s ticker name, Total daily volume (for the given year) and Returns (Annual in Percentage).

Fig.1 i.e. for the year 2017 shows positive returns for all but 1 of the 12 companies and can be quite deceiving since Fig.2 shows that the companies unanimously had negative returns.

In conclusion, based on annual returns alone we can deduce that these are extremely volatile stocks and a very risky investment.

### <ins>Total Daily Volume</ins> 

In general, high volumes of trading daily may show that a company’s stock is doing well.

Specific to the company DQ, its stocks had a low trading volume daily in 2017 but high annual returns. But just as in the previous scenario, in 2018 we see that there has been a complete flip and the opposite can be observed with a year-end of negative 63%. 

In conclusion, DQ stocks are definitely a very risky and even dangerous investment especially if Steve’s parents were to invest all their money into these since the analysis shows how despite high trading volume the outcomes have been negligible and not worth it.

## Summary

This data analysis helped develop an understanding of refactoring of code in VBA, and led me to see the advantages versus disadvantages for the same.

### What are the advantages or disadvantages of refactoring code?


<ins>*Advantages*</ins>:
The main advantage and reason is to better ones’ code and improve its quality.

- Organized and Readable: The code is easy to read and understand.

- Efficient and robust: 
By improving the logic of the code, we were able to reduce the run-time for it by more than 5x and allowing for quicker programming.

- Enhanced Functionality:
Refactoring allows for the detection of bugs — especially those that may have originally been missed.

<ins>*Disadvantages*</ins>:
The following are real life disadvantages, or rather limitations owing to the refactoring of code:

- Refactoring already running code may lead to new errors or bugs creeping into the code and therefore increase time consumed with now debugging it.

- When refactoring someone else’s’ code one cannot always ensure prior proper documentation which may lead to time taken to understand it before refactoring it and even then may not allow for the improvement in the best possible way.

### <ins>How do these pros and cons apply to refactoring the original VBA script</ins>?

- I enjoyed refactoring the code since while slightly frustrating initially, in retrospect it ended up with me having a much higher quality improved code with a run-time more than 5 times faster than the original code.

- I was able to understand and deeply delve into the logic of the code as I played with the loops and operators to improve said logic.

- I cannot overlook the fact that I was able to accomplish this because of the initially well written and clearly documented code. Practically speaking, one cannot improve ‘older’ code whether ones’ own or another’s without understanding it and can ultimately end up with errors and bugs (as did I initially but that was in part due to this being one of the first times I used VBA)
