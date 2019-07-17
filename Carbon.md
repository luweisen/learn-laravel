url:https://9iphp.com/web/laravel/php-datetime-package-carbon.html
use Illuminate\Support\Carbon;


https://carbon.nesbot.com/docs/

$dt = Carbon::create(2012, 1, 31, 0);

echo $dt->toDateTimeString();            // 2012-01-31 00:00:00

echo $dt->addCenturies(5);               // 2512-01-31 00:00:00
echo $dt->addCentury();                  // 2612-01-31 00:00:00
echo $dt->subCentury();                  // 2512-01-31 00:00:00
echo $dt->subCenturies(5);               // 2012-01-31 00:00:00

echo $dt->addYears(5);                   // 2017-01-31 00:00:00
echo $dt->addYear();                     // 2018-01-31 00:00:00
echo $dt->subYear();                     // 2017-01-31 00:00:00
echo $dt->subYears(5);                   // 2012-01-31 00:00:00

echo $dt->addQuarters(2);                // 2012-07-31 00:00:00
echo $dt->addQuarter();                  // 2012-10-31 00:00:00
echo $dt->subQuarter();                  // 2012-07-31 00:00:00
echo $dt->subQuarters(2);                // 2012-01-31 00:00:00

echo $dt->addMonths(60);                 // 2017-01-31 00:00:00
echo $dt->addMonth();                    // 2017-03-03 00:00:00 equivalent of $dt->month($dt->month + 1); so it wraps
echo $dt->subMonth();                    // 2017-02-03 00:00:00
echo $dt->subMonths(60);                 // 2012-02-03 00:00:00

echo $dt->addDays(29);                   // 2012-03-03 00:00:00
echo $dt->addDay();                      // 2012-03-04 00:00:00
echo $dt->subDay();                      // 2012-03-03 00:00:00
echo $dt->subDays(29);                   // 2012-02-03 00:00:00

echo $dt->addWeekdays(4);                // 2012-02-09 00:00:00
echo $dt->addWeekday();                  // 2012-02-10 00:00:00
echo $dt->subWeekday();                  // 2012-02-09 00:00:00
echo $dt->subWeekdays(4);                // 2012-02-03 00:00:00

echo $dt->addWeeks(3);                   // 2012-02-24 00:00:00
echo $dt->addWeek();                     // 2012-03-02 00:00:00
echo $dt->subWeek();                     // 2012-02-24 00:00:00
echo $dt->subWeeks(3);                   // 2012-02-03 00:00:00

echo $dt->addHours(24);                  // 2012-02-04 00:00:00
echo $dt->addHour();                     // 2012-02-04 01:00:00
echo $dt->subHour();                     // 2012-02-04 00:00:00
echo $dt->subHours(24);                  // 2012-02-03 00:00:00

echo $dt->addMinutes(61);                // 2012-02-03 01:01:00
echo $dt->addMinute();                   // 2012-02-03 01:02:00
echo $dt->subMinute();                   // 2012-02-03 01:01:00
echo $dt->subMinutes(61);                // 2012-02-03 00:00:00

echo $dt->addSeconds(61);                // 2012-02-03 00:01:01
echo $dt->addSecond();                   // 2012-02-03 00:01:02
echo $dt->subSecond();                   // 2012-02-03 00:01:01
echo $dt->subSeconds(61);                // 2012-02-03 00:00:00

echo $dt->addMilliseconds(61);           // 2012-02-03 00:00:00
echo $dt->addMillisecond();              // 2012-02-03 00:00:00
echo $dt->subMillisecond();              // 2012-02-03 00:00:00
echo $dt->subMillisecond(61);            // 2012-02-03 00:00:00

echo $dt->addMicroseconds(61);           // 2012-02-03 00:00:00
echo $dt->addMicrosecond();              // 2012-02-03 00:00:00
echo $dt->subMicrosecond();              // 2012-02-03 00:00:00
echo $dt->subMicroseconds(61);           // 2012-02-03 00:00:00

// and so on for any unit: millenium, century, decade, year, quarter, month, week, day, weekday,
// hour, minute, second, microsecond.

// Generic methods add/sub (or subtract alias) can take many different arguments:
echo $dt->add(61, 'seconds');                      // 2012-02-03 00:01:01
echo $dt->sub('1 day');                            // 2012-02-02 00:01:01
echo $dt->add(CarbonInterval::months(2));          // 2012-04-02 00:01:01
echo $dt->subtract(new DateInterval('PT1H'));      // 2012-04-01 23:01:01