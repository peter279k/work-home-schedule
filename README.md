# Working Home Schedule

![CI](https://github.com/peter279k/work-home-schedule/workflows/CI/badge.svg?branch=master)
[![Coverage Status](https://coveralls.io/repos/github/peter279k/work-home-schedule/badge.svg?branch=master)](https://coveralls.io/github/peter279k/work-home-schedule?branch=master)

## Introduction

This is about working home schedule to estimate current working status on specific date.

## Usage

This class depends on `Carbon::mixin` method.

The code snippets are as follows:

- Set `startDateStatus` about working from home or office.
- Set `csvPath` about specific CSV file path.
- Set `csvHead` about whether CSV file path has head.
- Loading calendar data, the calendar CSV format is available [here](tests/fixtures/2020_calendar.csv).

We assume that the `2020-04-06` is start date and working statuses are as follows:

- The start date status is `office`.

Get next working date about code snippets are as follows:

```php
require __DIR__ . '/vendor/autoload.php';

use Carbon\Carbon;
use Lee\WorkHomeSchedule;

$filePath = __DIR__ . '/tests/fixtures/2020_calendar.csv';

$workingHomeSchedule = new WorkHomeSchedule();
$workingHomeSchedule->startDateStatus = 'office'; // The default value is empty string
$workingHomeSchedule->csvPath = $filePath; // The default value is empty string
$workingHomeSchedule->csvHead = true; // The default value is true

$workingHomeSchedule = $workingHomeSchedule->loadCalendarData($filePath);

Carbon::mixin($this->workingHomeSchedule);
$currentDate = Carbon::create('2020-04-06');

$nextWorkingDate = $currentDate->nextWorkingDate();

$carbonDate = $nextWorkingDate['date']; // Carbon::class instance
$carbonDateString = (string)$nextWorkingDate['date']; // 2020-04-07 00:00:00
$workingStatus = $nextWorkingDate['status']; // home
```

Get previous working date about code snippets are as follows:

```php
require __DIR__ . '/vendor/autoload.php';

use Carbon\Carbon;
use Lee\WorkHomeSchedule;

$filePath = __DIR__ . '/tests/fixtures/2020_calendar.csv';

$workingHomeSchedule = new WorkHomeSchedule();
$workingHomeSchedule->startDateStatus = 'office'; // The default value is empty string
$workingHomeSchedule->csvPath = $filePath; // The default value is empty string
$workingHomeSchedule->csvHead = true; // The default value is true

$workingHomeSchedule = $workingHomeSchedule->loadCalendarData($filePath);

Carbon::mixin($this->workingHomeSchedule);
$currentDate = Carbon::create('2020-04-06');

$previousWorkingDate = $currentDate->previousWorkingDate();

$carbonDate = $previousWorkingDate['date']; // Carbon::class instance
$carbonDateString = (string)$previousWorkingDate['date']; // 2020-04-01 00:00:00
$workingStatus = $previousWorkingDate['status']; // home
```

Get next working dates with specific date ranges about code snippets are as follows:

```php
require __DIR__ . '/vendor/autoload.php';

use Carbon\Carbon;
use Lee\WorkHomeSchedule;

$filePath = __DIR__ . '/tests/fixtures/2020_calendar.csv';

$workingHomeSchedule = new WorkHomeSchedule();
$workingHomeSchedule->startDateStatus = 'office'; // The default value is empty string
$workingHomeSchedule->csvPath = $filePath; // The default value is empty string
$workingHomeSchedule->csvHead = true; // The default value is true
$workingHomeSchedule->workingDays = 2; // The default value is 1

$workingHomeSchedule = $workingHomeSchedule->loadCalendarData($filePath);

Carbon::mixin($this->workingHomeSchedule);
$currentDate = Carbon::create('2020-04-06');

$nextWorkingDates = $currentDate->nextWorkingDates(); // The array length is 2

$nextWorkingDates[0]['date'] // Carbon::class instance
(string)$nextWorkingDates[0]['date'] // 2020-04-07 00:00:00
$nextWorkingDates[0]['status'] // home

$nextWorkingDates[1]['date'] // Carbon::class instance
(string)$nextWorkingDates[1]['date'] // 2020-04-08 00:00:00
$nextWorkingDates[1]['status'] // office

```

Get previous working dates with date ranges about code snippets are as follows:

```php
require __DIR__ . '/vendor/autoload.php';

use Carbon\Carbon;
use Lee\WorkHomeSchedule;

$filePath = __DIR__ . '/tests/fixtures/2020_calendar.csv';

$workingHomeSchedule = new WorkHomeSchedule();
$workingHomeSchedule->startDateStatus = 'office'; // The default value is empty string
$workingHomeSchedule->csvPath = $filePath; // The default value is empty string
$workingHomeSchedule->csvHead = true; // The default value is true
$workingHomeSchedule->workingDays = 2; // The default value is 1

$workingHomeSchedule = $workingHomeSchedule->loadCalendarData($filePath);

Carbon::mixin($this->workingHomeSchedule);
$currentDate = Carbon::create('2020-04-06');

$previousWorkingDates = $currentDate->previousWorkingDates(); // array length is 2

$previousWorkingDates[0]['date'] // Carbon::class instance
(string)$previousWorkingDates[0]['date'] // 2020-04-01 00:00:00
$previousWorkingDates[0]['status'] // home

$previousWorkingDates[1]['date'] // Carbon::class instance
(string)$previousWorkingDates[1]['date'] // 2020-03-31 00:00:00
$previousWorkingDates[1]['status'] // office
```
