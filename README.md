![Tag](https://img.shields.io/github/v/tag/angel-one/go-scrub)
[![Go Report Card](https://goreportcard.com/badge/github.com/angel-one/go-scrub)](https://goreportcard.com/report/github.com/angel-one/go-scrub)
[![GoDoc](https://godoc.org/github.com/angel-one/go-scrub?status.svg)](https://godoc.org/github.com/angel-one/go-scrub)

# go-scrub

A scrubbing utility in Golang to mask sensitive fields from a struct prior to logging.

Since the scrubbing utility function needs to work on any Golang struct where its fields and members are known only at runtime, we can leverage ["reflect"](https://pkg.go.dev/reflect), a powerful package from the Golang standard library, to scrub sensitive fields at any level of a deeply nested structure recursively.

## Installation

```
go get github.com/angel-one/go-scrub@latest
```

## Usage

```go
import "github.com/angel-one/go-scrub"

// Have a struct with some sensitive fields.
type testScrub struct {
    Username string
    Password string
    Codes    []string
}

// Create a struct with some sensitive data.
T := &testScrub{
    Username: "administrator",
    Password: "my_secret_passphrase",
    Codes:    []string{"pass1", "pass2", "pass3"},
}

// Create empty instance of testScrub
emptyT := &testScrub{}

// Create a set of field names to scrub (default is 'password').
fieldsToScrub := map[string]FieldScrubOptioner{
    "password":  FieldScrubOpts{
		MaskingSymbol: "*",
	},
    "codes":      FieldScrubOpts{
		MaskingSymbol: "*",
		PartScrubConf: *NewPartScrubConf(true, 0, 10, 1, 0, 2),
	},
}

scrub.MaskLenVary = true

// Call the util API to get a JSON formatted string with scrubbed field values.
out := scrub.Scrub(emptyT, T, fieldsToScrub, scrub.JSONScrub)

// Log the scrubbed string without worrying about prying eyes!
log.Println(out)
// OUTPUT: {username:administrator Password:******************** Codes:[..... ..... .....]}
```

-   NOTE: Please reffer to `scrub_test.go` for all supported scenarios

## Contributing

Contributions are most welcome! Please create a new issue and link your PR to it.
