# scheduling-date-library-php

* The library mostly deals with date function. Methods defined here are used to get the date collection for defined date range, days of week, day of month and days of week of month.

--------------------

* Function returns collection of dates between provided from_date and to_date.
* @param date $from_date
* @param date $to_date
* @return array
    
function get_dates_daily($from_date, $to_date)

--------------------

* Function returns collection of dates for the provided week days collection between from_date and to_date.
* @param array $day_collection
* @param date $from_date
* @param date $to_date
* @return array

function get_dates_week_day($day_collection, $from_date, $to_date)

--------------------

* Function returns collection of dates for selected day of month (1 - 31) between from_date and to_Date.
* @param int $day_of_month
* @param date $from_date
* @param date $to_date
* @return array

function get_dates_month_day($day_of_month, $from_date, $to_date) 

--------------------

* Function returns collection of dates for given week days, repeat (1-4) week between from_date and to_date.
* @param array $week_day_collection
* @param int $repeat
* @param date $from_date
* @param date $to_date
* @return array

function get_dates_month_week_day($week_day_collection, $repeat, $from_date, $to_date) 
