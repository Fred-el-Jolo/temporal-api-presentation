---
# You can also start simply with 'default'
theme: ./theme
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Temporal ... now !!!
#info: |
#  Temporal usage in april 2025
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# Temporal API

La gestion des dates du futur... sans attendre !!!

<div class="abs-bl m-6 text-xl">
  <span>Fred Guillaume</span>
  <a href="https://github.com/Fred-el-Jolo/" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
  <a href="https://www.linkedin.com/in/fredericguillaume/" target="_blank" class="slidev-icon-btn">
    <carbon:logo-linkedin />
  </a>
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# Le temps, qu'est ce c'est ?

<v-click>

Mesure de <span v-mark.highlight.orange="1">durées</span>, planification d'<span v-mark.highlight.orange="1">événements</span> qui rythme notre vie quotidienne.

Notre expérience quotidienne du temps nous semble simple, avec une approche intuitive.

Mais cela reste une expérience très <span v-mark.underline.red="2">locale</span>, <span v-mark.underline.red="2">culturelle</span> et même parfois avec sa dose de <span v-mark.underline.red="2">politique</span> ; et donc compliquée à généraliser...

</v-click>
<v-click at="+2">

# Comment mesurer le temps ?

</v-click>
<v-click>

- En se basant sur des phénomènes <span v-mark.highlight.orange="4">périodiques</span> (jour, nuit, saisons etc...) ou <span v-mark.highlight.orange="4">quantiques</span> (atomes de quartz, de césium...)

</v-click>
<v-click at="+1">

# Pourquoi c'est important ?

</v-click>
<v-click>

- Synchroniser des systèmes est primordial pour la <span v-mark.underline.red="6">cohérence des données</span>
- Il suffit d'échanger avec des personnes sur d'autres fuseaux horaires pour se rendre compte de la complexité de la gestion du temps

</v-click>

---
transition: fade-out
---

# TAI, UT, UTC, DLS

<v-click>

- <span v-mark.underline.blue="1">TAI (Temps Atomique International):</span> basé sur les atomes de césium
> La seconde est la durée exacte de 9 192 631 770 oscillations (ou périodes) de la transition entre les niveaux hyperfins de l’état fondamental de l’atome de 133Cs (atome au repos T=0K).

</v-click>
<v-click>

- <span v-mark.underline.blue="2">UT1 (Universal Time):</span> basé sur l'observation d'objets celestes vis à vis de la rotation de la terre. Irrégulier.

</v-click>
<v-click>

- <span v-mark.underline.blue="3">UTC (Coordinated Universal Time):</span> Du fait de ses irrégularités, UT1 différe progressivement de TAI.

Afin de garder une consistence entre les deux, UTC a été introduite.

Il suffit alors d'ajouter / de supprimer des <span v-mark.highlight.orange="4">"secondes intercalaires"</span> au TAI pour rester à moins de 0.9s de UT1.

</v-click>
<v-click at="5">

- <span v-mark.underline.blue="5">DST (Daylight saving time):</span> Heure d'été / heure d'hiver.

</v-click>
---
transition: fade-out
---

# Fuseaux horaires

<img class="m-auto max-w-8/10" src="/aus-tz.png"/>

---
transition: fade-out
---

# Fuseaux horaires

<img class="m-auto max-w-8/10" src="/paris-sydney-offset.png" title="https://partir.ouest-france.fr/australie/villes/sydney/heure" />

---
transition: fade-out
---

# IANA TZ DB - Secondes intercalaires (Leap seconds)
https://www.iana.org/time-zones

<<< @/snippets/leap-seconds.txt#snippet

---
transition: fade-out
---

# IANA TZ DB - évolution du fuseau horaire en France
https://www.iana.org/time-zones

<<< @/snippets/europe.txt#snippet


---
transition: fade-out
---

# JS Date
Le JS a été créé dans la précipitation, et la gestion des dates n'y a pas coupé: l'implémentation créée en 1997 était une copie de l'implem `java.util.Date` [\[ref\]](https://developer.mozilla.org/en-US/blog/javascript-temporal-is-coming/#what_is_javascript_temporal)

<v-click>

Ces comportements rigides et non adaptés à une gestion des dates moderne sont compliqués à gérer

</v-click>

<v-clicks>

- une date est forcément associée à une heure (et un timestamp)
- une date est soit définie dans le fuseau horaire (=timezone) de l'utilisateur, soit en UTC
- Pas de support de calendriers non-grégoriens
- une date est mutable
- Les opérations sur les dates et les conversions vers d'autres timezone sont très compliquées à réaliser

</v-clicks>
---
transition: fade-out
---

# JS Date issues

```ts {monaco-run}{ showOutputAt:'1', editorOptions: { lineNumbers:'on'} }
// Output format
const jsSophia = new Date('2025-04-22T18:30:00');

// Test equality between getTimezoneOffset() from 2 different dates ?
console.log(jsSophia.getTimezoneOffset() === new Date().getTimezoneOffset());

```
<v-click>

`getTimezoneOffset()` est indépendant de la date !!!

</v-click>

---
transition: fade-out
---

# JS Date issues

```ts {monaco-run}{ showOutputAt:'1', editorOptions: { lineNumbers:'on'} }
// Output format
const jsSophia = new Date('2025-04-22T18:30:00');

// Weird APIs
console.log('getDay()', jsSophia.getDay());     // ok, get the day here
console.log('getDate()', jsSophia.getDate());   // Huh... get... the full date here ?

```

<v-click>

`getDay()` renvoie le jour de la semaine tandis que `getDate()` retourne le jour du mois !!!

</v-click>

---
transition: fade-out
---

# JS Date issues

```ts {monaco-run}{ showOutputAt:'1', editorOptions: { lineNumbers:'on'} }
// Output format
const jsSophia = new Date('2025-04-22T18:30:00');

console.log('getMonth()', jsSophia.getMonth()); // Starts at 0

```

<v-click>

`getMonth()` retourne le mois, en démarrant de 0 (janvier) !!!

</v-click>

---
transition: fade-out
---

# JS Date issues

```ts {monaco-run}{ showOutputAt:'1', editorOptions: { lineNumbers:'on'} }
// Augment given date by 2 months
const addTwoMonths = (date: Date) => {
  return new Date(date.setMonth(date.getMonth() + 2));
}

// Output format
const jsSophia13 = new Date('2025-04-22T18:30:00');
const jsSophia14 = addTwoMonths(jsSophia13);
console.log(jsSophia13, jsSophia14);

```

<v-click>

La date initiale a été modifiée !!!

</v-click>


---
transition: fade-out
---

# JS Date issues

```ts {monaco-run}{ showOutputAt:'1', editorOptions: { lineNumbers:'on'} }
// Augment given date by 2 months
const addTwoMonths = (date: Date) => {
  const clone = structuredClone(date);    // Clone the object now 
  return new Date(clone.setMonth(date.getMonth() + 2));
}

// Output format
const jsSophia13 = new Date('2025-04-22T18:30:00');
const jsSophia14 = addTwoMonths(jsSophia13);
console.log(jsSophia13, jsSophia14);

```

<v-click>

Fixed !!!

</v-click>


---
transition: fade-out
---

# JS Date issues

```ts {monaco-run}{ showOutputAt:'1', editorOptions: { lineNumbers:'on'} }
// Convert to another tz => date, not string
const jsSophia = new Date('2025-04-22T18:30:00');
const jsSophiaInNY = jsSophia.toLocaleString('en-US', {timeZone: 'America/New_York'});

console.log(jsSophia);
console.log(jsSophiaInNY);
//console.log(new Date(jsSophiaInNY));

```
<v-click>

`toLocaleString()` renvoie un `string`, qui ne peut pas être parsé tel quel

</v-click>


---
transition: fade-out
---



# Solution #01: utiliser des librairies (date-fns, moment...)

<v-clicks>

## Libraries basées sur Intl 
- Luxon (successor of Moment.js)
- date-fns-tz (extension for date-fns)
- Day.js (when using its Timezone plugin)



## Librairies non basées sur Intl Libraries (incluant leurs propres données de timezone)
- js-joda/timezone (extension for js-joda)
- moment-timezone (extension for Moment.js)
- date-fns-timezone (extension for older 1.x of date-fns)
- BigEasy/TimeZone
- tz.js

Mais... si on regardait ce qui arrive en standard ?

</v-clicks>

---
transition: fade-out
---

# Solution #02: temporal API
Temporal API... ou le proposal toujours dans le pipe, mais qui ne sort jamais...

<v-clicks>

- Repo lancé en 2017
- Atteint le stage 3 du TC39 en mars 2021
- Intégré sur Firefox 139 (nightly) le 10 avril 2025 !!! [\[ref\]](https://bugzilla.mozilla.org/show_bug.cgi?id=1912511#[object%20HTMLDivElement])

Hmmm... Mais alors, on va attendre encore pour basculer sur l'API ?

...

...

[Temporal github repo](https://github.com/tc39/proposal-temporal) => "Dev-ready" polyfills dispos, à utiliser avec précaution en prod...

</v-clicks>

---
transition: fade-out
---

# [fullcalendar/temporal-polyfill](https://github.com/fullcalendar/temporal-polyfill)

---
transition: fade-out
---

# Architecture Temporal API
L'API temporal gère plusieurs aspect du temps de manière séparée, sur des namespaces dédiés:

<v-clicks depth="2">

- Le calcul de durée `Temporal.Duration`
- La représentation d'un instant donné dans le temps
  - représentation "timestamp": `Temporal.Instant`
  - représentation d'une date & heure combinés à une timezone `Temporal.ZonedDateTime`
- La représentation des différentes composantes d'une date & heure sans timezone ("wall clock")
  - `Temporal.PlainDateTime`
  - `Temporal.PlainDate`
  - `Temporal.PlainTime`
  - `Temporal.PlainMonthDay`
  - `Temporal.PlainYearMonth`

</v-clicks>

---
transition: fade-out
---

<img class="m-auto max-w-8/10" src="/object-model.svg" />

---
transition: fade-out
---

<img class="m-auto max-w-8/10" src="/persistence-model.svg" />

<!--
# Calendars in temporal
https://tc39.es/proposal-temporal/docs/calendar-review.html
- less business centric, in-browser use cases where non ISO calendar more & more used
- emerging markets
- dev burden (i18n require platform & library support)
-->

---
transition: fade-out
---

# Temporal Duration
Les durées temporal peuvent être sérialisées via le format ISO 8601

<v-clicks depth="2">

- `±P nY nM nW nD T nH nM nS`
- Durées calendaires: année, mois, semaine
  - <span v-mark.box.orange="3">Non-portables</span>
- Durées non-calendaires: heure, minute, seconde...
  - <span v-mark.box.green="5">Portable, indépendantes du calendrier</span>

- Les méthodes `round()`, `total()`, `compare()` acceptent une option `relativeTo` afin de fournir les infos nécessaires à l'affichage de durée calendaires.

</v-clicks>

---
transition: fade-out
---

# Temporal perf benchmark
Attention aux perfs sur l'API.
[\[ref\]](https://bryntum.com/blog/javascript-temporal-is-it-finally-here/)

---
transition: fade-out
---


# Temporal playground - #1 durées

```ts {monaco-run}{ editorOptions: { lineNumbers:'on'} }
import { Temporal } from 'temporal-polyfill';

const jsSophia11 = Temporal.ZonedDateTime.from("2022-10-11T18:30:00[Europe/Paris]");
const jsSophia12 = Temporal.ZonedDateTime.from("2023-06-15T18:17:43[Europe/Paris]");
const jsSophia13 = Temporal.ZonedDateTime.from("2025-04-22T18:30:00[Europe/Paris]");

const jsSophia14 = jsSophia13.add(Temporal.Duration.from({months: 2}));

console.log(jsSophia13.since(jsSophia12)/*.toLocaleString()*/);

//console.log(jsSophia14);
//console.log(jsSophia13.since(jsSophia12).round({smallestUnit: 'second'}).toLocaleString());

```

<!--
console.log(jsSophia13.since(jsSophia12).round({
    relativeTo: jsSophia13,
    largestUnit: 'year',
    smallestUnit: 'second'
  }).toLocaleString());

console.log(jsSophia13.since(jsSophia12).add(jsSophia12.since(jsSophia11)).round({
    relativeTo: jsSophia13,
    largestUnit: 'year',
    smallestUnit: 'second'
  }).toLocaleString());

-->

---
transition: fade-out
---

# Temporal playground - #2 "Wall clock"

```ts {monaco-run}{ editorOptions: { lineNumbers:'on'} }
import { Temporal } from 'temporal-polyfill';

const heureMeeting = Temporal.PlainDateTime.from("2023-06-15T10:00:00");

// Convert to Paris timezone
//const heureMeetingParis = heureMeeting;

console.log(heureMeeting);
//console.log('Paris', heureMeetingParis);
```

<!--
const heureMeetingParis = heureMeeting.toZonedDateTime('Europe/Paris');

const heureMeetingNY = heureMeetingParis.withTimeZone('America/New_York');

const heureMeetingUTC = heureMeetingParis.toInstant();
-->


---
transition: fade-out
---

# Temporal playground - #3 Conversions de timezones

```ts {monaco-run}{ editorOptions: { lineNumbers:'on'} }
import { Temporal } from 'temporal-polyfill';

const heureMeeting = Temporal.PlainDateTime.from("2023-06-15T10:00:00");
const heureMeetingParis = heureMeeting.toZonedDateTime('Europe/Paris');

//const heureMeetingNY = heureMeetingParis;   // Use `with*` methods to change date components

//const heureMeetingUTC = heureMeetingParis;  // toInstant() returns an UTC date representing
                                              // an unique point in time

console.log(heureMeeting);
//console.log('Paris', heureMeetingParis);
//console.log('NY', heureMeetingNY);
//console.log('UTC', heureMeetingUTC);
//console.log(heureMeetingParis.offset, heureMeetingNY.offset, heureMeetingUTC);

```
<!--
const heureMeetingNY = heureMeetingParis.withTimeZone('America/New_York');

const heureMeetingUTC = heureMeetingParis.toInstant();
-->


---
transition: fade-out
---

# Temporal playground - #4 Calendriers

```ts {monaco-run}{ editorOptions: { lineNumbers:'on'} }
import { Temporal } from 'temporal-polyfill';

const gregorianDate = Temporal.Now.zonedDateTimeISO();

const chineseDate = gregorianDate.withCalendar("chinese");
console.log(gregorianDate);
console.log(chineseDate);
console.log(gregorianDate.toLocaleString('fr'));
console.log(chineseDate.toLocaleString('fr', {calendar: 'chinese'}));

```
<!--
islamic-umalqura
chinese

zh-CN
-->


---
transition: fade-out
---

# Temporal playground - #5 helpers divers

```ts {monaco-run}{ editorOptions: { lineNumbers:'on'} }
import { Temporal } from 'temporal-polyfill';

const newDate = Temporal.ZonedDateTime.from("2025-04-22T18:30:00[Europe/Paris]");


console.log(newDate);

```
<!--
inLeapYear
monthsInYear
daysInMonth

timezoneId
offset
-->


---
transition: fade-out
---

# Temporal playground - #6 heure d'été / hiver

```ts {monaco-run}{ editorOptions: { lineNumbers:'on'} }
import { Temporal } from 'temporal-polyfill';

//const before2007 = Temporal.ZonedDateTime.from("2006-02-01T00:00:00[America/New_York]");
const from2007 = Temporal.ZonedDateTime.from("2007-02-01T00:00:00[America/New_York]");


//console.log(before2007);
console.log(from2007);

```




---
transition: fade-out
layout: end
---


# Questions ?


<!--


You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<!--style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style-->

<!--
TAI: Temps Atomique International
UT: Universal Time
UTC: Coordinated Universal Time
# JS date issues:

- It doesn’t support Dates but only Datetimes (all date objects are unix timestamps).
- It doesn’t support time zones other than the user’s local time and UTC.
- The parser’s behaviour is inconsistent from one platform to another.
- The Date object is mutable.
- The behaviour of daylight saving time is unpredictable.
- No support for non-Gregorian calendars.
- No date arithmetic like add or subtract time.
https://medium.com/@raphael.moutard/handling-dates-in-javascript-the-wrong-way-d98cb2835200
https://medium.com/@raphael.moutard/why-programmers-are-so-bad-at-handling-time-part-1-01da6b50c141
https://medium.com/@raphael.moutard/why-programmers-are-so-bad-at-handling-time-part-2-b21aff190bfe


# Perfs benchmark
https://bryntum.com/blog/javascript-temporal-is-it-finally-here/


https://developer.mozilla.org/en-US/blog/javascript-temporal-is-coming/
=> browser status


# TZ db, data & code
https://data.iana.org/time-zones/tz-link.html
https://www.iana.org/time-zones
https://github.com/eggert/tz




# Detailed dates issues
https://toastui.medium.com/handling-time-zone-in-javascript-547e67aa842d


# Pays liés à une TZ
https://en.wikipedia.org/wiki/UTC+09:00



# https://maggiepint.com/category/date-proposal/

# Github temporal
https://github.com/tc39/proposal-temporal



1. Gestion du temps
2. JS Date API => relou !!!
3. Les libs, oui, du standard, ouiiiiiiiiiiiiiiiiiiiiiii !!!
4. statut temporal API
5. polyfill dispos
6. Temporal API - Archi globale
https://tc39.es/proposal-temporal/docs/
=> associés avec calendriers

7. Temporal API - ZonedDateTime

=> chinese calendar

8. Temporal API - PlainDateTime
9. Temporal API - Durations
Calcul entre sessions js sophia



// Getter & setters: local vs UTC
const nye = new Date('2024-12-31T23:00:00Z');
console.log(`year  local: ${nye.getFullYear()} utc: ${nye.getUTCFullYear()}`);
console.log(`month local: ${nye.getMonth()}    utc: ${nye.getUTCMonth()}`);
console.log(`day   local: ${nye.getDate()}     utc: ${nye.getUTCDate()}`);
console.log(`hours local: ${nye.getHours()}    utc: ${nye.getUTCHours()}`);


Intl-based Libraries

New development should choose from one of these implementations, which rely on the Intl API for their time zone data:

    Luxon (successor of Moment.js)
    date-fns-tz (extension for date-fns)
    Day.js (when using its Timezone plugin)

Non-Intl Libraries

These libraries are maintained, but carry the burden of packaging their own time zone data, which can be quite large.

    js-joda/timezone (extension for js-joda)
    moment-timezone* (extension for Moment.js)
    date-fns-timezone (extension for older 1.x of date-fns)
    BigEasy/TimeZone
    tz.js




# Calendars in temporal
https://tc39.es/proposal-temporal/docs/calendar-review.html
- less business centric, in-browser use cases where non ISO calendar more & more used
- emerging markets
- dev burden (i18n require platform & library support)

-->

<div class="abs-b m-6 text-sm">
<PoweredBySlidev mt-10 />
</div>


