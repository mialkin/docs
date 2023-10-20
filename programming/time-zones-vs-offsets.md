# Time zones vs offsets

A **time zone** is an area on a map.

An **offset** is the number of hours or minutes a certain time zone is ahead of or behind *UTC*.

A time zone's offset can change throughout the year because of *daylight saving time* or *summer time*. Sometimes laws change a time zone's offset or daylight savings pattern.

A **daylight saving time**, a term which is used in the United States, Canada, and Australia, and a **summer time**, a term which is used in Europe and other countries, is the practice of advancing clocks, typically by one hour, during warmer months so that darkness falls at a later clock time.

The **Coordinated Universal Time** or **UTC** is the primary time standard by which the world regulates clocks and time. UTC is not adjusted for daylight saving time. UTC is effectively a successor to Greenwich Mean Time (GMT).

[↑ Time Zones Aren't Offsets – Offsets Aren't Time Zones](https://spin.atomicobject.com/2016/07/06/time-zones-offsets)

[↑ Time zones of the world](https://en.wikipedia.org/wiki/Time_zone#/media/File:World_Time_Zones_Map.png)

## The `DateTimeOffset` structure in .NET

The `DateTimeOffset` structure represents a date and time value, together with an offset that indicates how much that value differs from UTC. Thus, the value always unambiguously identifies a single point in time.

The `DateTimeOffset` type includes all of the functionality of the `DateTime` type along with time zone awareness.

[↑ Choose between DateTime, DateTimeOffset, TimeSpan, and TimeZoneInfo](https://docs.microsoft.com/en-us/dotnet/standard/datetime/choosing-between-datetime#the-datetimeoffset-structure).

## Links

[↑ Joda Time Java library](https://www.joda.org/joda-time).

[↑ Noda Time .NET library](https://nodatime.org/).

[↑ Entity Framework Community Standup - Noda Time](https://www.youtube.com/watch?v=ZLJLfImuFqM).

[↑ What's wrong with DateTime anyway?](https://blog.nodatime.org/2011/08/what-wrong-with-datetime-anyway.html)

[↑ Just store UTC? Handling Time Zones & Daylight Saving](https://www.youtube.com/watch?v=Bxf_HMs7SeQ).
