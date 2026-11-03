<p align="center"><img src=".github/logo.png" alt="sink" width="160"></p>

# sink

[![Go Reference](https://pkg.go.dev/badge/github.com/jakobii/sink.svg)](https://pkg.go.dev/github.com/jakobii/sink)
[![Test](https://github.com/jakobii/sink/actions/workflows/test.yml/badge.svg?branch=main)](https://github.com/jakobii/sink/actions/workflows/test.yml)
[![Go Report Card](https://goreportcard.com/badge/github.com/jakobii/sink)](https://goreportcard.com/report/github.com/jakobii/sink)

Synchronization primitives for Go - "everything but the kitchen sink". A collection of context-aware, channel-based sync utilities that extend the standard library's `sync` package.

```
go get github.com/jakobii/sink
```

## Features

- 📦 **Drop-in replacements** for `sync.Mutex`, `sync.WaitGroup`
- 👀 **Channel-based API** that works with `select` statements
- 🫡 **Zero dependencies** beyond Go standard library

## Usage

The standard library's `sync` package is excellent for most use cases and provides the foundation for Go's concurrency model. However, there are scenarios where additional capabilities become valuable. When coordinating long-running operations or building systems that need to respect contexts and timeouts, the ability to cancel blocking operations becomes essential.

For integration with `select` statements:

```go
var mu sink.Mutex
select {
case mu.Acquire() <- sink.Lock:
	defer mu.Unlock()
	fmt.Println("acquired lock")
case <-time.After(someDuration):
	return errTimeout
}
```

```go
var wg sink.WaitGroup
wg.Go(func(){
	fmt.Printf("doing some work")
})
select {
case <-wg.Await():
	fmt.Println("work completed")
case <-time.After(someDuration):
	return errTimeout
}
```

See more [examples](./example_test.go).

## Credits

The Go gopher was designed by [Renée French](https://reneefrench.blogspot.com/) and is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
