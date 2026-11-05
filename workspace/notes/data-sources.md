# Data sources

## Open-Meteo (api.open-meteo.com/v1/forecast)

Probed on 2026-11-05 for Brussels, with `minutely_15=temperature_2m,cloud_cover,shortwave_radiation`. No API key.

- **Local times do not follow daylight saving.** With `timezone=Europe/Brussels`, a response has one
  `utc_offset_seconds` (3600 in November), and its naive times over 2026-10-25 run 01:45, 02:00 ... 03:15 with no
  repeated hour, although Brussels changed from +02:00 to +01:00 in that range. Ask `timeformat=unixtime`, leave
  `timezone` out (the response then says offset 0, GMT), and compute local times with the site's zone.
- `forecast_minutely_15=100` gives 100 values. The first is the quarter hour in progress (13:30Z when asked at
  13:30Z), so a slot that starts after now needs a few more values than slots: ask for more and cut.
- `shortwave_radiation` is a mean over the 15 minutes before its time, `shortwave_radiation_instant` the value at
  its time: at 07:00Z the mean was 4.0 with instants of 0 at 06:45Z and 10.6 at 07:00Z. So a mean stamped T belongs
  to the slot that starts at T-15min.
- The docs say that 15-minute data is real only in Central Europe and North America, and interpolated from hourly
  data elsewhere.
- `start_minutely_15` and `end_minutely_15` accept past dates, which gives a recorded response across a DST change.
  `past_days=92` is accepted (8928 values, back to 2026-08-05), which M3's solar factor can use.
- `grid.slots_in_range` starts at the first boundary at or after its start, whereas M2.5's slots start after now: when
  now is exactly on a boundary, that slot is not one of the 96.
- A bad request gives HTTP 400 and `{"error":true,"reason":"Latitude must be in range of -90 to 90°. Given: 95.0."}`.

## Prometheus HTTP API (known behaviour, not checked here: no server is reachable)

- `GET /api/v1/query_range?query=&start=&end=&step=` answers `{"status":"success","data":{"resultType":"matrix",
  "result":[{"metric":{...},"values":[[<unix time>,"<value as text>"],...]}]}}`. A step without data is simply absent
  from `values`, and a value may be the text `NaN` or `+Inf`.
- A request of more than 11,000 steps per series is refused, about 114 days at 15 minutes: split long ranges.
- A range query evaluates the expression at each step. A bare gauge gives the last sample at that instant, looking
  back up to 5 minutes, not an average. `avg_over_time(x[15m])` at T averages over the 15 minutes before T, which is
  what the capacity tariff measures, so evaluate at the slot's end. Counters take `rate(x[15m])`.
