Orders = DISTINCTCOUNT ( Fact_Orders[Order Id] )

Delivered Orders =
CALCULATE ( [Orders], Fact_Orders[Is Canceled] = FALSE () )

On-Time Orders =
CALCULATE ( [Orders], Fact_Orders[Is Canceled] = FALSE (), Fact_Orders[OTD Flag] = 1 )

Late Orders = [Delivered Orders] - [On-Time Orders]

OTD % = DIVIDE ( [On-Time Orders], [Delivered Orders] )

OTD Target = 0.95 -- better: a What-if parameter

OTD vs Target (pp) = ( [OTD %] - [OTD Target] ) \* 100

Avg Delay Days (Late) =
CALCULATE (
AVERAGE ( Fact_Orders[Delay Days] ),
Fact_Orders[OTD Flag] = 0,
Fact_Orders[Is Canceled] = FALSE ()
)

OTD % PY = CALCULATE ( [OTD %], SAMEPERIODLASTYEAR ( Dim_Date[Date] ) )

OTD YoY (pp) = ( [OTD %] - [OTD % PY] ) \* 100

OTD % Rolling 3M =
CALCULATE ( [OTD %], DATESINPERIOD ( Dim_Date[Date], MAX ( Dim_Date[Date] ), -3, MONTH ) )

Revenue at Risk =
CALCULATE ( SUM ( Fact_Orders[Sales] ), Fact_Orders[OTD Flag] = 0, Fact_Orders[Is Canceled] = FALSE () )
