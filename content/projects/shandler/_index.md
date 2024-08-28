---
name: "shandler"
description: "SLOG Handler with more knobs"
repo: "https://github.com/jordan-rash/slog-handler"
packages: ["disorder.dev/shandler"]
---

# sHandler - SLOG Handler with more knobs
[![Go Report Card](https://goreportcard.com/badge/disorder.dev/shandler)](https://goreportcard.com/report/disorder.dev/shandler)
[![Go Reference](https://pkg.go.dev/badge/disorder.dev/shandler.svg)](https://pkg.go.dev/disorder.dev/shandler)

I really like the ease of Go's `log/slog` package, but the default handlers left much to be desired...by me anyway. So I wrote my own

### _Installation_
```bash
go get disorder.dev/shandler
```

### _Features_

##### WithJSON
Enables JSON output for the log message.  This is useful for structured logging.

##### WithLogLevel
Controls the log level for the message.  This is useful for filtering messages.

##### WithTimeFormat
Controls the time format for the messages.

##### WithTextOutputFormat
This is a format string that gets used in text based logs.  It takes 3 strings: time, level, and message (in that order).  Include a newline at the end of your string.

##### WithStdOut
Controls which `io.Writer` is used for non-error log messages.

##### WithStdErr 
Controls which `io.Writer` is used for error messages.

##### WithColor
Adds color to the log levels in text mode

##### With{Trace|Debug|Info|Warn|Error|Fatal}Color
Overrides the default color for the log level.

##### WithShortLevels
Prints 3 character log levels instead of the full name.  In text mode, this helps keep the log lines visually straight.

## Examples

```go 
logger = slog.New(shandler.NewHandler(
	shandler.WithLogLevel(slog.LevelDebug),
	shandler.WithTimeFormat(time.RFC822),
	shandler.WithTextOutputFormat("%s | %s | %s\n"),
	shandler.WithStdErr(os.Stdout),
))
logger.With(slog.String("app", "myapp")).Debug("test")
```

#### Trace/Fatal Log Level
Library includes an easier way to log trace and fatal messages.  This is useful for debugging chatty logs.
```golang
ctx := context.TODO()
logger = slog.New(shandler.NewHandler(
	shandler.WithLogLevel(shandler.LevelTrace),
))
logger.Log(ctx, shandler.LevelTrace, "trace test")
logger.Log(ctx, shandler.FatalTrace, "fatal test") // this would appear in StdErr
```
