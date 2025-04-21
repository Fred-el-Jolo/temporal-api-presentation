---
# You can also start simply with 'default'
theme: seriph
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

Mesure de durées, planification d'événements qui rythme notre vie quotidienne.

Notre expérience quotidienne du temps nous semble simple, avec une approche intuitive.

Mais cela reste une expérience très locale, culturelle et même parfois avec sa dose de politique ; et donc compliquée à généraliser...

</v-click>

<v-click>

# Comment mesurer le temps ?
- En se basant sur des phénomènes périodiques (jour, nuit, saisons etc...) ou quantiques (atomes de quartz, de césium...)

</v-click>

<v-click>

# Pourquoi c'est important ?
- Synchroniser des systèmes est primordial pour la cohérence des données
- Il suffit d'échanger avec des personnes sur d'autres fuseaux horaires pour se rendre compte de la complexité de la gestion du temps

</v-click>

---
transition: fade-out
---

# TAI, UT, UTC, DLS
- TAI (Temps Atomique International): basé sur les atomes de césium
> La seconde est la durée exacte de 9 192 631 770 oscillations (ou périodes) de la transition entre les niveaux hyperfins de l’état fondamental de l’atome de 133Cs (atome au repos T=0K).

- UT1 (Universal Time): basé sur l'observation d'objets celestes vis à vis de la rotation de la terre. Irrégulier.
- UTC (Coordinated Universal Time): Du fait de ses irrégularités, UT1 différe progressivement de TAI.

Afin de garder une consistence entre les deux, UTC a été introduite.

Elle consiste à ajouter / supprimer des "secondes intercalaires au TAI pour rester à moins de 0.9s de UT1.
- DST (Daylight saving time): Heure d'été / heure d'hiver.

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
Le JS a été créé dans la précipitation, et la gestion des dates n'y a pas coupé: l'implémentation JS créée en 1997 était une copie de l'implem `java.util.Date` [\[ref\]](https://developer.mozilla.org/en-US/blog/javascript-temporal-is-coming/#what_is_javascript_temporal)

Ces comportements rigides et non adaptés à une gestion des dates moderne sont compliqués à gérer
- une date est forcément associée à une heure (et un timestamp)
- une date est soit définie dans le fuseau horaire (=timezone) de l'utilisateur, soit en UTC
- Pas de support de calendriers non-grégoriens
- une date est mutable
- Les opérations sur les dates et les conversions vers d'autres timezone sont très compliquées à réaliser

---
transition: fade-out
---


# JS Date issues (1/2)

```ts {monaco-run}
// Output format
const jsSophia = new Date('2025-04-22T18:30:00');

// 1.
// getTimezoneOffset is date-value independant !!!
console.log('getTimezoneOffset on 2 different dates', jsSophia.getTimezoneOffset() === new Date().getTimezoneOffset());

// 2.
// Weird APIs
console.log('getDay()', jsSophia.getDay());     // Day of week
console.log('getDate()', jsSophia.getDate());   // Day of month

// 3.
console.log('getMonth()', jsSophia.getMonth()); // Starts at 0
```

---
transition: fade-out
---

# JS Date issues (2/2)

```ts {monaco-run}
// 1.a
const addTwoMonths = (date) => {
  // 1.b
  //TODO hide this !!!! const clone = structuredClone(date);
  const clone = structuredClone(date);
  return new Date(clone.setMonth(date.getMonth() + 2));
}

// Output format
const jsSophia13 = new Date('2025-04-22T18:30:00');
const jsSophia14 = addTwoMonths(jsSophia13);
console.log(jsSophia13, jsSophia14);

// 2.
// Convert to another tz => date, not string
const jsSophia = new Date('2025-04-22T18:30:00');
const jsSophiaInNY = jsSophia.toLocaleString('en-US', {timeZone: 'America/New_York'});
console.log(jsSophiaInNY);
console.log(new Date(jsSophiaInNY));
```


---
transition: fade-out
---

# Solution #01: use libraries (date-fns, moment...)
## Intl-based Libraries
- Luxon (successor of Moment.js)
- date-fns-tz (extension for date-fns)
- Day.js (when using its Timezone plugin)

## Non-Intl Libraries (include their time zone data)
- js-joda/timezone (extension for js-joda)
- moment-timezone* (extension for Moment.js)
- date-fns-timezone (extension for older 1.x of date-fns)
- BigEasy/TimeZone
- tz.js

Mais... si on regardait ce qui arrive en standard ?

---
transition: fade-out
---

# Solution #02: temporal API
Temporal API... ou le proposal toujours dans le pipe, mais qui ne sort jamais...
- Repo lancé en 2017
- Atteint le stage 3 du TC39 en mars 2021
- Activé sur Firefox 139 (nightly) le 10 avril 2025 !!! [\[ref\]](https://bugzilla.mozilla.org/show_bug.cgi?id=1912511#[object%20HTMLDivElement])

Hmmm... Mais alors, on va attendre encore pour basculer sur l'API ?

[Temporal github repo](https://github.com/tc39/proposal-temporal)
"Dev-ready" polyfills dispos, à utiliser avec précaution en prod...

---
transition: fade-out
---

# [fullcalendar/temporal-polyfill](https://github.com/fullcalendar/temporal-polyfill)

---
transition: fade-out
---

# Architecture Temporal API
L'API temporal gère plusieurs aspect du temps de manière séparée, sur des namespaces dédiés:
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


---
transition: fade-out
---

<img src="/object-model.svg"  width=400/>

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

<img src="/persistence-model.svg"  width=400/>

---
transition: fade-out
---

# Temporal Duration
Les durées temporal peuvent être sérialisées via le format ISO 8601
`±P nY nM nW nD T nH nM nS`

- Durées calendaires: année, mois, semaine
  - Non-portables
- Durées non-calendaires: heure, minute, seconde...
  - Portable, indépendantes du calendrier

- Les méthodes `round()`, `total()`, `compare()` acceptent une option `relativeTo` afin de fournir les infos nécessaires à l'affichage de durée calendaires.

---
transition: fade-out
---

# Temporal playground


```ts {monaco-run}
import { Temporal } from 'temporal-polyfill';

//console.log(Temporal.Now.zonedDateTimeISO().toString());

const jsSophia11 = Temporal.ZonedDateTime.from(
  "2022-10-11T18:30:00[Europe/Paris]",
);

const jsSophia12 = Temporal.ZonedDateTime.from(
  "2023-06-15T18:30:00[Europe/Paris]",
);

const jsSophia13 = Temporal.ZonedDateTime.from(
  "2025-04-22T18:30:00[Europe/Paris]",
);

const jsSophia14 = jsSophia13.add(Temporal.Duration.from({months: 2}));

const heureMeeting = Temporal.PlainDateTime.from(
  "2023-06-15T10:00:00",
);

const heureMeetingParis = heureMeeting.toZonedDateTime('Europe/Paris');

const heureMeetingNY = heureMeetingParis.withTimeZone('America/New_York');
const heureMeetingUTC = heureMeetingParis.toInstant();

//console.log(jsSophia13.since(jsSophia12).toLocaleString());
//console.log(jsSophia13.since(jsSophia12).round({relativeTo: jsSophia13, smallestUnit: 'minute',  largestUnit: 'year'}).toLocaleString());
//console.log(jsSophia14);
console.log(heureMeetingParis.offset, heureMeetingNY.offset, heureMeetingUTC);

const zdt = Temporal.ZonedDateTime.from(
  "2021-07-01T12:34:56[America/New_York]",
);
const newZDT = zdt.withCalendar("islamic-umalqura");
//console.log(newZDT.toLocaleString("en-US", { calendar: "islamic-umalqura" }));
```

---
transition: fade-out
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

<PoweredBySlidev mt-10 />
