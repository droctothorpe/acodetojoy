---
title: "Leveled logs with Cobra and Logrus"
date: "2019-07-23"
hideReadMore: true
Description: How to implement leveled logs for a Cobra CLI using the Logrus framework.
---

I recently wrote a CLI using [Cobra](https://github.com/spf13/cobra), a widely adopted CLI framework for Golang that's used in Kubectl, Docker, Terraform, Hugo, and other industry-standard CLIs. 

I wanted to use [Logrus](https://github.com/sirupsen/logrus), a highly regarded structured logging framework for Go. I wanted informative, colorized, leveled logs, but I also wanted to be able to log simple, user-facing output without all of the bells and whistles. I was surprised at the lack of documentation around this simple goal.  

Here's one way to solve this problem.

Create a file called `logging.go` in your `cmd` package:

```go
package cmd

import (
	"fmt"
	log "github.com/sirupsen/logrus"
	"github.com/spf13/cobra"
)

var debug bool

type PlainFormatter struct {
}

func (f *PlainFormatter) Format(entry *log.Entry) ([]byte, error) {
	return []byte(fmt.Sprintf("%s\n", entry.Message)), nil
}
func toggleDebug(cmd *cobra.Command, args []string) {
	if debug {
		log.Info("Debug logs enabled")
		log.SetLevel(log.DebugLevel)
		log.SetFormatter(&log.TextFormatter{})
	} else {
		plainFormatter := new(PlainFormatter)
		log.SetFormatter(plainFormatter)
	}
}
```

Add a `PreRun` to each of your Cobra commands:
```go
var addCmd = &cobra.Command{
    ...
	PreRun: toggleDebug, // This. 
	Run: func(cmd *cobra.Command, args []string) {
    // Subcommand logic goes here.
    }
}
```

Add a global persistent flag to `root.go`:
```go
func init() {
    ...
	rootCmd.PersistentFlags().BoolVarP(&debug, "debug", "d", false, "verbose logging")
}
```

It's as simple as that. Now you can reap the benefits of flag-toggled leveled logs. Use the following invocations anywhere in your code:
```golang
log.Info()
log.Debug()
log.Warning()
log.Error()
log.Fatal()
```

Debug logs are suppressed by default. You can pass the `--debug` or `-d` flag to any command to print more verbose, colorized debug logs, which are super helpful when troubleshooting. 

I hope you found this helpful.
