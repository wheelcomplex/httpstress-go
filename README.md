## httpstress-go
httpstress-go is a CLI interface for
[httpstress](https://github.com/chillum/httpstress.git) library.

Use it for stress testing of HTTP servers with many concurrent connections.

### Installation
* Install [Go runtime](http://golang.org/doc/install)
* `go get github.com/chillum/httpstress-go`
* Ready to use: launch `httpstress-go` with desired options

### Options
* `-c NUM` -- concurrent connections number (defaults to 1000)
* `-m NUM` -- total connections number (optional)
* `URL list` -- URLs to fetch

### Example usage
`httpstress-go -c 1000 -m 2000 http://localhost http://google.com`
