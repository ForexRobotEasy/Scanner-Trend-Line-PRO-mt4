# Scanner Trend Line PRO mt4

Scanner Trend Line PRO mt4 is a MetaTrader 4 indicator developed by the Forex Robot Easy Team (forexroboteasy.com). This indicator is designed to provide professional traders with accurate and reliable trend line signals.

## Indicator handle

```mql5
int indicator_handle;
```

## Global variables

```mql5
bool use_set_files = true;
string set_file_directory = 'C:\\set_files\\';
```

## Symbol and timeframe settings

```mql5
string symbol = '';
ENUM_TIMEFRAMES timeframe = PERIOD_M5;
```

## Trading functions

```mql5
bool open_position(double lot_size, ENUM_ORDER_TYPE order_type, double stop_loss, double take_profit);
void close_position();
void modify_position(double stop_loss, double take_profit);
```

## OnStart function

The `OnStart` function is the main function that executes when the indicator is started. It initializes the Trend Line PRO indicator and enters a loop to check for new signals.

```mql5
void OnStart()
{
   //--- Initialize the Trend Line PRO indicator
   if (!IndicatorInitialize())
   {
      Print('Failed to initialize the Trend Line PRO indicator!');
      return;
   }
   
   //--- Main loop
   while (!IsStopped())
   {
      //--- Check for new signals from the indicator
      if (IndicatorSignalReceived())
      {
         //--- Get the signal details
         double lot_size = IndicatorGetLotSize();
         ENUM_ORDER_TYPE order_type = IndicatorGetOrderType();
         double stop_loss = IndicatorGetStopLoss();
         double take_profit = IndicatorGetTakeProfit();
         
         //--- Open a new position
         if (!open_position(lot_size, order_type, stop_loss, take_profit))
         {
            Print('Failed to open position!');
         }
      }
      
      //--- Sleep for 1 second before checking for new signals
      Sleep(1000);
   }
}
```

## IndicatorInitialize function

The `IndicatorInitialize` function initializes the Trend Line PRO indicator by checking if it is installed and getting the indicator handle.

```mql5
bool IndicatorInitialize()
{
   //--- Check if the indicator is installed
   if (!IndicatorIsInstalled())
   {
      Print('Trend Line PRO indicator is not installed!');
      return false;
   }
   
   //--- Get the indicator handle
   indicator_handle = iCustom(symbol, timeframe, 'Trend Line PRO');
   
   //--- Check if the indicator handle is valid
   if (indicator_handle == INVALID_HANDLE)
   {
      Print('Failed to get the indicator handle!');
      return false;
   }
   
   return true;
}
```

## IndicatorIsInstalled function

The `IndicatorIsInstalled` function checks if the Trend Line PRO indicator is installed by checking if it is available in the Market.

```mql5
bool IndicatorIsInstalled()
{
   string indicator_name = 'Trend Line PRO';
   
   //--- Check if the indicator is available in the Market
   if (!MarketIsPresent(indicator_name))
   {
      Print(indicator_name + ' is not available in the Market!');
      return false;
   }
   
   return true;
}
```

## IndicatorSignalReceived function

The `IndicatorSignalReceived` function checks if a new signal is received from the Trend Line PRO indicator by comparing the number of counted bars with the total number of bars.

```mql5
bool IndicatorSignalReceived()
{
   //--- Check if the indicator signal buffer has been updated
   return IndicatorCounted(indicator_handle) < Bars;
}
```

## IndicatorGetLotSize function

The `IndicatorGetLotSize` function gets the lot size from the Trend Line PRO indicator.

```mql5
double IndicatorGetLotSize()
{
   //--- Get the lot size from the indicator
   return iCustom(symbol, timeframe, 'Trend Line PRO', 1, 0);
}
```

## IndicatorGetOrderType function

The `IndicatorGetOrderType` function gets the order type from the Trend Line PRO indicator.

```mql5
ENUM_ORDER_TYPE IndicatorGetOrderType()
{
   //--- Get the order type from the indicator
   return (ENUM_ORDER_TYPE)iCustom(symbol, timeframe, 'Trend Line PRO', 2, 0);
}
```

## IndicatorGetStopLoss function

The `IndicatorGetStopLoss` function gets the stop loss from the Trend Line PRO indicator.

```mql5
double IndicatorGetStopLoss()
{
   //--- Get the stop loss from the indicator
   return iCustom(symbol, timeframe, 'Trend Line PRO', 3, 0);
}
```

## IndicatorGetTakeProfit function

The `IndicatorGetTakeProfit` function gets the take profit from the Trend Line PRO indicator.

```mql5
double IndicatorGetTakeProfit()
{
   //--- Get the take profit from the indicator
   return iCustom(symbol, timeframe, 'Trend Line PRO', 4, 0);
}
```

## open_position function

The `open_position` function opens a new position by placing an order with the specified lot size, order type, stop loss, and take profit.

```mql5
bool open_position(double lot_size, ENUM_ORDER_TYPE order_type, double stop_loss, double take_profit)
{
   //--- Place the order
   bool order_placed = OrderSend(symbol, order_type, lot_size, 0, 0, stop_loss, take_profit);
   
   //--- Check if the order was placed successfully
   if (!order_placed)
   {
      Print('Failed to place the order!');
      return false;
   }
   
   return true;
}
```

## close_position function

The `close_position` function closes the current position by closing the order associated with it.

```mql5
void close_position()
{
   //--- Close the position
   bool position_closed = PositionClose(PositionGetTicket());
   
   //--- Check if the position was closed successfully
   if (!position_closed)
   {
      Print('Failed to close the position!');
   }
}
```

## modify_position function

The `modify_position` function modifies the current position by modifying the stop loss and take profit levels of the order associated with it.

```mql5
void modify_position(double stop_loss, double take_profit)
{
   //--- Modify the position
   bool position_modified = OrderModify(PositionGetTicket(), PositionGetOpenPrice(), stop_loss, take_profit, 0);
   
   //--- Check if the position was modified successfully
   if (!position_modified)
   {
      Print('Failed to modify the position!');
   }
}
```

For detailed reviews and trading results of this product, please visit [Forex Robot Easy](https://forexroboteasy.com/forex-robot-review/review-scanner-trend-line-pro-mt4-forex-software-for-professional-traders/). Please note that ForexRobotEasy is not the official developer of this product. We only provide a sample code that can work as described in this product. To find the official developer of this product, please use MQL5.
