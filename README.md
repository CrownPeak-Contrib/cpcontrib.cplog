# cpcontrib.cplog
CPLog for NLog style logging within CrownPeak

```
paket install cpcontrib.cplog
```

CPLog is designed to be similar to NLog logging framework (see NLog).

We've also included two default loggers for sending Email and using CrownPeak's Util.Log sink.

## EmailLogger

EmailLogger sends a email with all the log messages after calling Flush or Dispose.  Best to use a using statement to ensure Dispose is called.

```c#
<%
  Log = new EmailLogger("");
  //Log.IsDebugEnabled = true; //use this to increase logging level to Debug
  
  try
  {
    //code here
  }
  finally
  {
    Log.Dispose();
  }
%>
```

## UtilLogLogger

The UtilLogLogger utilizes the Util.Log method for recording log entries to history.  Two options, to log into a specific Asset's history or the System's history.  

```c#
//UtilLogLogger can utilize current asset, or log to System Log (no asset) by using other constructor
Log UtilLogLogger = new UtilLogLogger(asset);  //Log entries go into asset's history

Log UtilLogLogger = new UtilLogLogger(); //Log entries go into System history
```

When using the Buffered version (by default), setting Buffered=true, all the log messages are stored up until Flush or Dispose are called.  Setting Buffered=false will cause every Log message to be immediately run through the Util.Log underlying method.  This will produce a chatty history, and might be what is desired.
```c#
Log UtilLogLogger = new UtilLogLogger(asset) { Buffered = false };

Log.Debug("Test");
Log.Info("Info Test"); //will see this go immediately into History
```
