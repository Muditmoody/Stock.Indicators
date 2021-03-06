﻿# Chandelier Exit

Chandelier Exit is typically used for stop-loss and can be computed for both long or short types.
[More info ...](https://school.stockcharts.com/doku.php?id=technical_indicators:chandelier_exit)

```csharp
// usage
IEnumerable<ChandelierResult> results = Indicator.GetChandelier(history, lookbackPeriod, multiplier, type);  
```

## Parameters

| name | type | notes
| -- |-- |--
| `history` | IEnumerable\<[Quote](../../GUIDE.md#quote)\> | Historical Quotes data should be at any consistent frequency (day, hour, minute, etc).  You must supply at `N+1` periods worth of `history`.
| `lookbackPeriod` | int | Number of periods (`N`) for the lookback evaluation.  Default is 22.
| `multiplier` | decimal | Multiplier number must be a positive value.  Default is 3.
| `type` | ChandelierType | Direction of exit.  See [ChandelierType options](#chandeliertype-options) below.  Default is `ChandelierType.Long`.

### ChandelierType options

| type | description
|-- |--
| `ChandelierType.Long` | Intended as stop loss value for long positions. (default)
| `ChandelierType.Short` | Intended as stop loss value for short positions.

## Response

```csharp
IEnumerable<ChandelierResult>
```

The first `N` periods will have `null` Chandelier values since there's not enough data to calculate.  We always return the same number of elements as there are in the historical quotes.

### ChandelierResult

| name | type | notes
| -- |-- |--
| `Date` | DateTime | Date
| `ChandelierExit` | decimal | Exit line

## Example

```csharp
// fetch historical quotes from your favorite feed, in Quote format
IEnumerable<Quote> history = GetHistoryFromFeed("SPY");

// calculate Chandelier(22,3,LONG)
IEnumerable<ChandelierResult> results = Indicator.GetChandelier(history,22,3,ChandelierType.Long);

// use results as needed
DateTime evalDate = DateTime.Parse("12/31/2018");
ChandelierResult result = results.Where(x=>x.Date==evalDate).FirstOrDefault();
Console.WriteLine("ChandelierExit(22,3) on {0} was ${1}", result.Date, result.ChandelierExit);
```

```bash
ChandelierExit(22,3) on 12/31/2018 was 255.09
```
