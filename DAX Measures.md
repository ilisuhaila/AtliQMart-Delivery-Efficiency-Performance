# [DAX] AtliQMart Delivery Efficiency Performance

#### Total Order Lines 
        
        = COUNT(fact_order_lines[order_id])
        
#### LIFR % 

        = DIVIDE(
            SUM(
              fact_order_lines[In Full]),
            [Total Order Lines],
            0)
         
#### VOFR % 

        = DIVIDE(
            SUM(
                fact_order_lines[delivery_qty]),
            SUM(
                fact_order_lines[order_qty]),
            0)

#### Total Orders

        = DISTINCTCOUNT(fact_orders_aggregate[order_id])

#### OT % 

        = DIVIDE(
            SUM(
                fact_orders_aggregate[on_time]),
            [Total Orders])

#### IF % 

        = DIVIDE(
            SUM(
                fact_orders_aggregate[in_full]),
            [Total Orders])

#### OTIF % 

        = DIVIDE(
            SUM(
                fact_orders_aggregate[otif]),
            [Total Orders])

#### OT Target % 

        = AVERAGE(dim_targets_orders[ontime_target%])
          *0.01

#### OT Target Gap 

        = [OT %] - [OT Target %]    

#### IF Target % 

        = AVERAGE(dim_targets_orders[infull_target%])
          *0.01

#### IF Target Gap 

        = [IF %] - [IF Target %]             

#### OTIF Target % 

        = AVERAGE(dim_targets_orders[otif_target%])
          *0.01

#### OTIF Target Gap 

        = [OTIF %] - [OTIF Target %]

#### Delay Count 

        = CALCULATE(
            COUNTROWS(fact_order_lines),
            fact_order_lines[actual_delivery_date] > fact_order_lines[agreed_delivery_date])

#### Delayed Days 

        = MAXX(
            fact_order_lines,
            DATEDIFF(
                fact_order_lines[agreed_delivery_date],
              fact_order_lines[actual_delivery_date],
              DAY))

#### Total Product Delivered 

        = SUM(fact_order_lines[delivery_qty])

#### Total Product Ordered 

        = SUM(fact_order_lines[order_qty])        
