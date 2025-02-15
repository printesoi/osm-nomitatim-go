# OSM Nominatim API for Golang

The `nominatim` package provides functions to interact with the v1 API of the OpenStreetMap Nominatim web service. It includes functionalities for reverse geocoding, retrieving details about specific OSM objects, searching for places, and looking up information based on OSM type and ID.

This is a fork of [turistikrota/osm](https://github.com/turistikrota/osm) which
adds some missing fields in the `ReverseResult` struct (the result of the
reverse geocoding call).

## Installation

To install the package, use:

```sh
go get github.com/printesoi/osm-nominatim-go
```

## Usage

### Importing the Package

```go
import "github.com/printesoi/osm-nominatim-go"
```

### Methods

- `Reverse(ctx context.Context, lat float64, long float64, opts ...optFunc) (*ReverseResult, error)`
- `Details(ctx context.Context, osmType string, osmID int, opts ...optFunc) (*DetailsResult, error)`
- `DetailsWithPlaceID(ctx context.Context, placeID int, opts ...optFunc) (*DetailsResult, error)`
- `Search(ctx context.Context, q string, opts ...optFunc) ([]SearchResult, error)`
- `Lookup(ctx context.Context, osmType string, osmID int, opts ...optFunc) (*LookupResult, error)`

### Internationalization

The package supports internationalization. To set the language for the API requests, use the `WithLanguage` option:

```go
result, err := osm.Reverse(ctx, 40.748817, -73.985428, osm.WithLocale("de"))
```

or change the default language:

```go
osm.DefaultConfig = osm.Config{
    Locale: "de",
}
```

### Examples

#### Reverse Geocoding

```go
package main

import (
    "context"
    "fmt"
    nominatim "github.com/printesoi/osm-nominatim-go"
)

func main() {
    ctx := context.Background()
    result, err := nominatim.Reverse(ctx, 40.748817, -73.985428) // Latitude and Longitude for the Empire State Building
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println("Reverse Geocoding Result:", result)
}
```

#### Retrieving Details

```go
package main

import (
    "context"
    "fmt"
    nominatim "github.com/printesoi/osm-nominatim-go"
)

func main() {
    ctx := context.Background()
    result, err := nominatim.DetailsWithPlaceID(ctx, 240109189) // PlaceID for the Empire State Building
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println("Details Result:", result)
}
```

#### Searching for Places

```go
package main

import (
    "context"
    "fmt"
    nominatim "github.com/printesoi/osm-nominatim-go"
)

func main() {
    ctx := context.Background()
    results, err := nominatim.Search(ctx, "Central Park")
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    for _, result := range results {
        fmt.Println("Search Result:", result)
    }
}
```

#### Looking Up Information

```go
package main

import (
    "context"
    "fmt"
    nominatim "github.com/printesoi/osm-nominatim-go"
)

func main() {
    ctx := context.Background()
    result, err := nominatim.Lookup(ctx, "way", 123456789) // Example OSM type and ID
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println("Lookup Result:", result)
}
```

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
