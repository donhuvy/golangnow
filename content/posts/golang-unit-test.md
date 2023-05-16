---
categories:
  - Golang
tags:
  - Golang
  - Go
  - Unit test
title: "Golang Unit Test"
date: 2023-04-26T14:26:20+07:00
draft: false
---

Development environment: Go 1.20.3 , Windows 11 x64 , JetBrains GoLand 2023.1 . Use command for init new module

```bash
go mod init vy_learn_aws
```

Directory structure

![directory.png](/img/go_unit_test/directory_test.png)

Naming function convention

![test_convention.png](/img/go_unit_test/test_convention.png)


## Part 1: Golang unit testing with mathematics operators

Create file `math/math.go`

```go
package math

func Add(x, y int) int {
	return x + y
}

func Subtract(x, y int) int {
	return x - y
}

func Divide(x, y int) float64 {
	if y == 0 {
		return float64(0)
	}
	return float64(x / y)
}

func Multiply(x, y int) int {
	return x * y
}
```

Create file `math/math_test.go`

```go
package math

import "testing"

type AddData struct {
	x, y   int
	result int
}

func TestAdd(t *testing.T) {
	testData := []AddData{
		{1, 2, 3},
		{3, 5, 8},
		{7, -4, 3},
	}
	for _, datum := range testData {
		result := Add(datum.x, datum.y)
		if result != datum.result {
			t.Errorf("Add(%d, %d) FAILED. Expected %d got %d\n", datum.x, datum.y, datum.result, result)
		}
	}
}

func TestDivide(t *testing.T) {
	result := Divide(5, 0)
	if result != 0 {
		t.Errorf("Divide(5, 0) FAILED. Expected %f, got %f\n", 0.0, result)
	} else {
		t.Logf("Divide(5,0) PASSED. Expected %f, got %f\n", 0.0, result)
	}
}
```

## Part 2: Golang unit testing with HTTP request (standard library)

File `server/server.go`

```go
package server

import (
	"encoding/json"
	"net/http"
)

type Weather struct {
	City     string `json:"city"`
	Forecast string `json:"forecast"`
}

func GetWeather(url string) (*Weather, error) {
	resp, err := http.Get(url)
	if err != nil {
		return nil, err
	}
	defer resp.Body.Close()
	var weather Weather
	err = json.NewDecoder(resp.Body).Decode(&weather)
	if err != nil {
		return nil, err
	}
	return &weather, nil
}
```

File `server/server_test.go`

```go
package server

import (
	"errors"
	"net/http"
	"net/http/httptest"
	"reflect"
	"testing"
)

type Tests struct {
	name          string
	server        *httptest.Server
	response      *Weather
	expectedError error
}

func TestGetWeather(t *testing.T) {
	tests := []Tests{
		{
			name: "basic-request",
			server: httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
				w.WriteHeader(http.StatusOK)
				w.Write([]byte(`{"city": "Denver, CO", "forecast": "sunny"}`))
			})),
			response: &Weather{
				City:     "Denver, CO",
				Forecast: "sunny",
			},
			expectedError: nil,
		},
	}

	for _, test := range tests {
		t.Run(test.name, func(t *testing.T) {
			defer test.server.Close()
			resp, err := GetWeather(test.server.URL)
			if !reflect.DeepEqual(resp, test.response) {
				t.Errorf("FAILED: expected %v, got %v\n", test.response, resp)
			}
			if !errors.Is(err, test.expectedError) {
				t.Errorf("Expected error FAILED: expected %v got %v\n", test.expectedError, err)
			}
		})
	}
}
```

Run unit test

```go
API server listening at: 127.0.0.1:53259
=== RUN   TestGetWeather
=== RUN   TestGetWeather/basic-request
--- PASS: TestGetWeather (0.00s)
    --- PASS: TestGetWeather/basic-request (0.00s)
PASS


Debugger finished with the exit code 0
```

## Part 3: Test generating string

File `basic/foo/foo.go`

```go
package foo

import "fmt"

func Foo() string {
	return fmt.Sprintf("Foo")
}
```

File `basic/foo/foo_test.go`

```go
package foo

import "testing"

func TestFoo(t *testing.T) {
	expected := "Foo"
	actual := Foo()
	if expected != actual {
		t.Errorf("Expected %s do not match actual %s", expected, actual)
	}
}
```

Result

```go
API server listening at: 127.0.0.1:53518
=== RUN   TestFoo
--- PASS: TestFoo (0.00s)
PASS


Debugger finished with the exit code 0
```

## Part 4: Library support Golang unit testing

https://github.com/stretchr/testify

See coverage with percent

```bash
PS C:\Users\admin\GolandProjects\vy_learn_aws> go test .\basic\foo\ --coverprofile=vy.out           
ok      vy_learn_aws/basic/foo  0.234s  coverage: 100.0% of statements
```

![cover.png](/img/go_unit_test/cover.png)
