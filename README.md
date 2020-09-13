# MMM-AVStock
MagicMirror module for displaying stock price using the Alphavantage API.


## Screenshot
- `mode:table`  
![ScreenShot for Table](https://raw.githubusercontent.com/lavolp3/MMM-AVStock/master/avstock-table.PNG)

- `mode:ticker`  
![ScreenShot for Ticker](https://raw.githubusercontent.com/lavolp3/MMM-AVStock/master/avstock-ticker.PNG)

- `mode:grid` with `direction:'row'`   
![ScreenShot for Ticker](https://raw.githubusercontent.com/lavolp3/MMM-AVStock/master/avstock-grid.PNG)

- `mode:ticker with own purchase prices`  
![ScreenShot for Ticker](https://raw.githubusercontent.com/spitzlbergerj/MMM-AVStock/master/avstock-ticker-purchasePrices.jpg) 

- `mode:ticker with own purchase prices and total performance compared to the purchase price`  
![ScreenShot for Ticker](https://raw.githubusercontent.com/spitzlbergerj/MMM-AVStock/master/avstock-ticker-purchase-performace.jpg) 


## UPDATES ##

** spitzlbergerj, 20200913 **
- performance can now also be displayed relatively by using the option showPerformanceAbsolute

** 2.2.0 **
- (by spitzlbergerj) within the ticker mode, a line with the own purchase price and the display for profit and loss is added. The performance compared to the own purchase price can be displayed too.
- changed width scheme, hopefully fixing #35 
- optimized tagline, now showing once below chart or table/grid/ticker
- code cleanup
- fixed ticker length (may fix ticker length issue): ticker can now expand over many items.
- optimized grid


** 2.1.0 **
- grid layout
- direction option to show chart besides other module
- improved styling
- fixed alias issue


** 2.0.0 **
- included Highcharts npm module for charts
- option to add chart (`mode: series` still available)
- improved API calls
- technicals (EMA or SMA)
- touch functions (choose stock chart, zoom in chart)

## Installation
```shell
cd ~/MagicMirror/modules
git clone https://github.com/lavolp3/MMM-AVStock
cd MMM-AVStock
npm install
```

## Alphavantage Key
Get your free API key here:
https://www.alphavantage.co/

Free account has a limit of quota (5 request per minute, 500 requests per day).  
Over the time Alphavantahge has shown to be unreliable, since more and more stocks and function got excluded.  
Currently several users (including me) only get stock data for the previous day.   
I am working on an alternative API.  



## Configuration

### Simple

```javascript
{
  //disabled:true,
  module: "MMM-AVStock",
  position: "top_left",
  config: {
    apiKey : "YOUR_ALPHAVANTAGE_KEY",
    symbols : ["AAPL", "GOOGL", "TSLA"],
    alias: ["APPLE", "GOOGLE", "TESLA"],
  }
},
```

### Detailed

```javascript
{
    module: "MMM-AVStock",
    position: "top_right", //"bottom_bar" is better for `mode:ticker`
    config: {
        apiKey : "",
        timeFormat: "DD-MM HH:mm",
        width: '100%',
        symbols : ["AAPL", "GOOGL", "TSLA"],
        alias: ["APPLE", "GOOGLE", "TESLA"],
        locale: config.language,
        tickerDuration: 20,
        chartDays: 90,
        mode : "table",                  // "table" or "ticker"
        showChart: true,
        chartWidth: null,
        showVolume: true,
        chartInterval: "daily",          // choose from ["intraday", "daily", "weekly", "monthly"]
        movingAverage: {
            type: 'SMA',
            periods: [200]
        },
        decimals : 2,
        chartType: 'line',                // 'line', 'candlestick', or 'ohlc'
        chartLineColor: '#eee',
        chartLabelColor: '#eee',
        coloredCandles: true,
        debug: false
    }
},
```

### mode ticker with own purchase prices

```javascript
{
    module: "MMM-AVStock",
    position: "bottom_bar",
    config: {
        apiKey : "",{
        mode : "ticker",
        symbols : ["TL0.F","AMZN","MSFT"],
        alias: ["Tesla","Amazon","Microsoft"],
        purchasePrice: [123.45, 1234.56, 12.34],
        decimals: 0,
        tickerDuration: 20,
        showChart: false,
        showVolume: false,
        showPurchasePrices: true,
        showPerformance2Purchase: true,
        showPerformanceAbsolute: true,
    }
},
```

## Configuration Options

| **Option** | **Type** | **Default** | **Description** |
| --- | --- | --- | --- |
| `api_key` | string | '' | Your API Key obtained from <https://www.alphavantage.co/> (limited to 500 requests a day)|
| `mode` | string | 'table' | Use 'table' for table mode, 'ticker' for ticker mode, or 'grid' for a grid mode. |
| `width` | integer | 400 | Width of every module element. Sets the distinctive width of table, ticker, chart, or grid. |
| `classes` | string | 'small' | Set classes known from module classes (xsmall, small, bright, dimmed etc.) |
| `direction` | string | 'row' | You can set `row` or `column`. setting row will place chart next to the table/grid/ticker as long as there is enough space available |
| `timeFormat` | string | 'DD-MM HH:mm' | Format of dates to be shown. Use moment.js format style here |
| `symbols` | array | ["AAPL", "GOOGL", "TSLA"] | Array of stock symbols |
| `alias` | array | [] | Array of aliases to replace the stock symbol. Leave all or each empty to show the symbol name. |
| `purchasePrice` | array | [123.45, 123.45, 123.45] | Array of own purchase prices |
| `showPurchasePrices` | boolean | false | Whether to show the own purchase prices. |
| `showPerformance2Purchase` | boolean | false | Whether to show the total performace compared to the own purchase prices. |
| `showPerformanceAbsolute` | boolean | false | Whether to show the total performace as absolute or relative value. |
| `locale` | string | config.locale | Locale to convert numbers to the respective number format. |
| `tickerDuration` | integer | 20 | Determines ticker speed |
| `chartDays` | integer | 90 | Number of days to show in the chart. (Max 90 days!) |
| `showChart` | boolean | true | Whether to show the chart. |
| `chartWidth` | integer | null | Determines width of chart, separate from overall width above |
| `chartInterval` | string | 'daily' | Chart interval. Currently only daily supported! |
| `showVolume` | boolean | true | Show volume bars in the chart. |
| `movingAverage` | object | `{ type: "SMA", periods: [200]}`  | movingAverages to include in the graph. Use `EMA` or `SMA` type and an array of all moving averages you want to see. Consider that every MA uses an own API call. |
| `decimals` | integer |  | Number of decimals. |
| `chartType` | string | `line` | Use `line`, `candlestick`, or `ohlc` |
| `chartLineColor` | string | `#eee` | Color of line chart |
| `chartLabelColor` | string | `#eee` | Color of chart labels |
| `coloredCandles` | boolean | true | Whether to use colored candles or OHLC bars. |
| `debug` | false | Debug mode: additional output on server side (console) and client side (browser) |


## ToDo

[ ] Use another API!
[ ] Grid view
