# scheduling-date-library-php
/**
 * Description of Dateslib
 * This library mostly deals with date function.
 * Methods defined here are used to get the date collection for
 * defined date range, days of week, day of month and days of week of month.
 *
 * @author Darshan
 * @created 23/03/2016
 */
class Scheduling {

    /**
     * Function returns collection of dates between provided from_date and to_date.
     * @param date $from_date
     * @param date $to_date
     * @return array
     */
    function get_dates_daily($from_date, $to_date) {
        $data_result_collection = array();
        $from_date_timestamp = strtotime($from_date);
        $to_date_timestamp = strtotime($to_date);

        while ($from_date_timestamp <= $to_date_timestamp) {
            $data_result_collection[] = date("Y-m-d", $from_date_timestamp);
            $from_date_timestamp = strtotime("+1 day", $from_date_timestamp);
        }
        return $data_result_collection;
    }

    /**
     * Function returns collection of dates for the provided week days collection between from_date and to_date.
     * @param array $day_collection
     * @param date $from_date
     * @param date $to_date
     * @return array
     */
    function get_dates_week_day($day_collection, $from_date, $to_date) {
        $from_date_timestamp = strtotime($from_date);
        $to_date_timestamp = strtotime($to_date);
        $date_result_collection = array();

        $days_selected = array();
        foreach ($day_collection as $day) {
            $days_selected[$day] = $from_date_timestamp;
        }

        while (count($days_selected) > 0) {
            foreach ($days_selected as $day => $time_stamp) {
                $next_day = "next " . $day;
                $date_time_stamp = strtotime("$next_day", $time_stamp);

                if ($date_time_stamp <= $to_date_timestamp) {
//                    $date_result_collection[date('Y-m-d', $date_time_stamp)] = date("d", $date_time_stamp);
                    $date_result_collection[] = date('Y-m-d', $date_time_stamp);
                    $days_selected[$day] = $date_time_stamp;
                } else {
                    unset($days_selected[$day]);
                }
            }
        }
        usort($date_result_collection, array($this, "sort_date_array"));
        return $date_result_collection;
    }

    /**
     * Function returns collection of dates for selected day of month (1 - 31) between from_date and to_Date.
     * @param int $day_of_month
     * @param date $from_date
     * @param date $to_date
     * @return array
     */
    function get_dates_month_day($day_of_month, $from_date, $to_date) {
        $from_date_timestamp = strtotime($from_date);
        $to_date_timestamp = strtotime($to_date);
        $result_date_collection = array();

        $date_collection = array();
        while ($from_date_timestamp <= $to_date_timestamp) {
            $date_collection[date('Y-m-d', $from_date_timestamp)] = date('d', $from_date_timestamp);
            $from_date_timestamp = strtotime('+1 day', $from_date_timestamp);
        }

        foreach ($date_collection as $date => $day) {
            if ($day == $day_of_month) {
                $result_date_collection[] = $date;
            }
        }

        return $result_date_collection;
    }

    /**
     * Function returns collection of dates for given week days, repeat (1-4) week between from_date and to_date.
     * @param array $week_day_collection
     * @param int $repeat
     * @param date $from_date
     * @param date $to_date
     * @return array
     */
    function get_dates_month_week_day($week_day_collection, $repeat, $from_date, $to_date) {

        $from_date_timestamp = strtotime($from_date);
        $to_date_timestamp = strtotime($to_date);
        $days_selected_timestamp = array();
        $result_date_collection = array();

        foreach ($week_day_collection as $day) {
            $days_selected_timestamp[$day] = strtotime("$repeat $day of this month", $from_date_timestamp);
        }

        while (count($days_selected_timestamp) > 0) {
            foreach ($days_selected_timestamp as $day => $day_timestamp) {
                if ($day_timestamp > $to_date_timestamp)
                    unset($days_selected_timestamp[$day]);
                else {
                    if ($day_timestamp >= $from_date_timestamp) {
                        $result_date_collection[] = date('Y-m-d', strtotime("$repeat $day of this month", $day_timestamp));
                    }
                    $days_selected_timestamp[$day] = strtotime("+1 month", $day_timestamp);
                }
            }
        }

        return $result_date_collection;
    }

    /**
     * Function returns the array sorted dates in assending order.
     * @param array $a
     * @param array $b
     * @return array
     */
    private function sort_date_array($a, $b) {
        return strtotime($a) - strtotime($b);
    }
