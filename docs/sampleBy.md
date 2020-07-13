---
id: sampleBy
title: SAMPLE BY
sidebar_label: SAMPLE BY
---

`SAMPLE BY` is used on time-series data to summarise large datasets into
aggregates of homogeneous time chunks as part of a
[SELECT statement](sqlSELECT.md).

:::note
To use `SAMPLE BY`, one column needs to be designated as `timestamp`.
Find out more in the **[designated timestamp](designatedTimestamp.md)** section.
:::

### Syntax

![sample by syntax](/static/img/doc/diagrams/sampleBy.svg)

WHere `SAMPLE_SIZE` is the unit of time by which you wish to aggregate your
results, and `n` is the number of time-chunks that will be summarised together.

### Examples

Assume the following table

```shell script
TRADES
===============================================
timestamp,    buysell,    quantity,     price
-----------------------------------------------
ts1           B           q1            p1
ts2           S           q2            p2
ts3           S           q3            p3
...           ...         ...           ...
tsn           B           qn            pn
```

The following will return the number of trades per hour:

```sql title="trades - hourly interval"
SELECT timestamp, count()
FROM TRADES
SAMPLE BY 1h;
```

The following will return the trade volume in 30 minute intervals

```sql title="trades - 30 minute interval"
SELECT timestamp, sum(quantity*price)
FROM TRADES
SAMPLE BY 30m;
```

The following will return the average trade notional (where notional is = q \*
p) by day:

```sql title="trades - daily interval"
SELECT timestamp, avg(quantity*price)
FROM TRADES
SAMPLE BY 1d;
```