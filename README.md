# moment-jalaali

A Jalaali (Jalali, Persian, Khorshidi, Shamsi) calendar system plugin for moment.js.

[![NPM](https://img.shields.io/npm/v/moment-jalaali.svg)](https://www.npmjs.com/package/moment-jalaali)
[![Build Status](https://travis-ci.org/jalaali/moment-jalaali.png?branch=master)](https://travis-ci.org/jalaali/moment-jalaali)

Jalali calendar is a solar calendar that was used in Persia, variants of which today are still in use in Iran as well as Afghanistan. [Read more on Wikipedia](http://en.wikipedia.org/wiki/Jalali_calendar) or see [Calendar Converter](http://www.fourmilab.ch/documents/calendar/).

This plugin adds Jalaali calendar support to [momentjs](http://momentjs.com) library.

Calendar conversion is based on the [algorithm provided by Kazimierz M. Borkowski](http://www.astro.uni.torun.pl/~kb/Papers/EMP/PersianC-EMP.htm) and has a very good performance.

## Note (Feb 2022)

If you just need to display date and time in Persian calendar, you may use `Intl` which is ECMAScript Internationalization API with a [very good browser support](https://caniuse.com/mdn-javascript_builtins_intl_datetimeformat_format). For example:

```js
const d = new Date(2022,2,21)

// Simple format
console.log(new Intl.DateTimeFormat('fa-IR').format(d));
// => ۱۴۰۱/۱/۱

// Full long format
console.log(new Intl.DateTimeFormat('fa-IR', {dateStyle: 'full', timeStyle: 'long'}).format(d));
// => ۱۴۰۱ فروردین ۱, دوشنبه، ساعت ۰:۰۰:۰۰ (‎+۳:۳۰ گرینویچ)

// Latin numbers
console.log(new Intl.DateTimeFormat('fa-IR-u-nu-latn', {dateStyle: 'full', timeStyle: 'long'}).format(d));
// => 1401 فروردین 1, دوشنبه، ساعت 0:00:00 (‎+3:30 گرینویچ)

// English US locale with Persian calendar
console.log(new Intl.DateTimeFormat('en-US-u-ca-persian', {dateStyle: 'full', timeStyle: 'long'}).format(d));
// => Monday, Farvardin 1, 1401 AP at 12:00:00 AM GMT+3:30

// Just year
console.log(new Intl.DateTimeFormat('en-US-u-ca-persian', {year: 'numeric'}).format(d));
// => 1401 AP

// Just month
console.log(new Intl.DateTimeFormat('en-US-u-ca-persian', {month: 'short'}).format(d));
// Farvardin

// Just day
console.log(new Intl.DateTimeFormat('en-US-u-ca-persian', {day: 'numeric'}).format(d));
// => 1
```

## Where to use it

Like `momentjs`, `moment-jalaali` works in browser and in Node.js.

### Node.js

```shell
npm install moment-jalaali
```

```js
var moment = require('moment-jalaali')
moment().format('jYYYY/jM/jD')
```

### Browser

You may use the `node_modules/build/moment-jalaali.js` file.

```html
<script src="node_modules/moment/min/moment.min.js"></script>
<script src="node_modules/moment-jalaali/build/moment-jalaali.js"></script>
<script>
  moment().format('jYYYY/jM/jD')
</script>
```

## API

This plugin tries to mimic `momentjs` api. Basically, when you want to format or parse a string, just add a `j` to the format token like 'jYYYY' or 'jM'. For example:

```js
m = moment('1360/5/26', 'jYYYY/jM/jD') // Parse a Jalaali date
m.format('jYYYY/jM/jD [is] YYYY/M/D') // 1360/5/26 is 1981/8/17

m.jYear() // 1360
m.jMonth() // 4
m.jDate() // 26
m.jDayOfYear() // 150
m.jWeek() // 22
m.jWeekYear() // 1360

m.add(1, 'jYear')
m.add(2, 'jMonth')
m.add(3, 'day')
m.format('jYYYY/jM/jD') // 1361/7/29

m.jMonth(11)
m.startOf('jMonth')
m.format('jYYYY/jM/jD') // 1361/12/1

m.jYear(1392)
m.startOf('jYear')
m.format('jYYYY/jM/jD') // 1392/1/1

m.subtract(1, 'jYear')
m.subtract(1, 'jMonth')
m.format('jYYYY/jM/jD') // 1390/12/1

moment('1391/12/30', 'jYYYY/jMM/jDD').isValid() // true (leap year)
moment('1392/12/30', 'jYYYY/jMM/jDD').isValid() // false (common year)
moment.jIsLeapYear(1391) // true
moment.jIsLeapYear(1392) // false

moment.jDaysInMonth(1395, 11) // 30
moment.jDaysInMonth(1394, 11) // 29

moment('1392/6/3 16:40', 'jYYYY/jM/jD HH:mm').format('YYYY-M-D HH:mm:ss') // 2013-8-25 16:40:00

moment('2013-8-25 16:40:00', 'YYYY-M-D HH:mm:ss').endOf('jMonth').format('jYYYY/jM/jD HH:mm:ss') // 1392/6/31 23:59:59

// Complex parse:
moment('1981 5 17', 'YYYY jM D').format('YYYY/MM/DD') // 1981/07/17
```

To add Persian language, use loadPersian method:

```js
moment.loadPersian([options])
```

### Options

| Param            | Type    | Default   | Description                               | Example                                      |
| ---------------- | ------- | -------   | ----------------------------------------  | -------------------------------------------- |
| usePersianDigits | Boolean | `false`   | Use persian digits (Use at your own risk) | `moment.loadPersian({usePersianDigits: true})` |
| dialect *        | String  | `persian` | Available values = `persian`, `persian-modern` | `moment.loadPersian({dialect: 'persian-modern'})` |

*use dialect option to change `usePersian` dialect, available options are:

* persian: default dialect(امرداد، آدینه، ...)

* persian-modern: modern dialect(مرداد، جمعه، ...)

#### in case getting error `'humanize' of undefined`

you should also require locale fa

```js
require('moment/locale/fa')
```

### React Native

To use `fromNow()` in React Native projects:

```js
import moment from "moment-jalaali";
import fa from "moment/src/locale/fa";
moment.locale("fa", fa);
moment.loadPersian();
```


