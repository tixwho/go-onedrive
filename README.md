# go-onedrive

![Test Status](https://github.com/goh-chunlin/go-onedrive/workflows/Go/badge.svg?branch=main)

go-onedrive is a Go client library for accessing the [Microsoft OneDrive REST API](https://docs.microsoft.com/en-us/onedrive/developer/rest-api/?view=odsp-graph-online).

This project is inspired by a few open-source projects, especially the [go-github project from Google](https://github.com/google/go-github).

Currently, **go-onedrive requires Go version 1.15 or greater**.  go-onedrive tracks
[Go's version support policy](https://golang.org/doc/devel/release.html#policy). I'll do my best not to break
older versions of Go if we don't have to, but due to tooling constraints, we
don't always test older versions.

## Usage ##

```go
import "github.com/goh-chunlin/go-onedrive/onedrive"
```

Construct a new OneDrive client, then use the various services on the client to access different parts of the OneDrive API. For example:

```go
ctx := context.Background()
ts := oauth2.StaticTokenSource(
	&oauth2.Token{AccessToken: "..."},
)
tc := oauth2.NewClient(ctx, ts)

client := onedrive.NewClient(tc)

// list all OneDrive drives for the current logged in user
drives, err := onedrive.Drives.List(ctx)
```

NOTE: Using the [context](https://godoc.org/context) package, one can easily
pass cancelation signals and deadlines to various services of the client for
handling a request. In case there is no context available, then `context.Background()`
can be used as a starting point.

### Authentication ###

The go-onedrive library does not directly handle authentication. Instead, when
creating a new client, pass an `http.Client` that can handle authentication for
you. The easiest and recommended way to do this is using the [oauth2](https://github.com/golang/oauth2)
library.

Note that when using an authenticated Client, all calls made by the client will
include the specified OAuth token. Therefore, authenticated clients should
almost never be shared between different users.

See the [oauth2 docs](https://godoc.org/golang.org/x/oauth2) for complete instructions on using that library.

## Contributing ##

This library is being initially developed as a library for my personal project, so API methods will likely be implemented 
in the order that they are needed by my personal project. However, I still welcome you to contribute to this project to 
support the following features.

- [ ] Drives
 - [x] Get Default Drive
 - [ ] Get Drive
 - [x] List all available drives
- [ ] Items
 - [ ] Create
 	- [ ] Create folder
 - [ ] Copy
 	- [ ] Copy file/folder
 	- [ ] Async job to track progress
 - [ ] Delete
 - [ ] Download
 - [x] List children
 - [ ] Search
 - [ ] Move
 - [ ] Upload
 	- [ ] Simple item upload <100MB
 	- [ ] Resumable item upload
 	- [ ] Upload from URL

## Sensei Projects ##

Special thanks to the following projects for providing useful references to me on this library.
- [google/go-github](https://github.com/google/go-github);
- [ggordan/go-onedrive](https://github.com/ggordan/go-onedrive).

## License ##

This library is distributed under the GPL-3.0 License found in the [LICENSE](./LICENSE) file.