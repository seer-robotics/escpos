# About escpos #

This is a [Golang](http://www.golang.org/project) package that provides
[ESC-POS](https://en.wikipedia.org/wiki/ESC/P) library functions to help with
sending control codes to a ESC-POS thermal printer.

## Installation ##

Install the package via the following:

    go get -u github.com/seer-robotics/escpos

## Usage ##

The escpos package can be used as the following:

```go
package main

import (
    "bufio"
    "net"

    "github.com/seer-robotics/escpos"
)

func main() {
    socket, err := net.Dial("tcp", "192.168.2.210:9100")
      if err != nil {
      println(err.Error())
    }
    defer socket.CLose()

    w := bufio.NewWriter(socket)
    p := escpos.New(w)

    p.Verbose = true

    p.Init()
    p.SetFontSize(2, 3)
    p.SetFont("A")
    p.Write("test1")
    p.SetFont("B")
    p.Write("test2")

    p.SetEmphasize(1)
    p.Write("hello")
    p.Formfeed()

    p.SetUnderline(1)
    p.SetFontSize(4, 4)
    p.Write("hello")

    p.SetReverse(1)
    p.SetFontSize(2, 4)
    p.Write("hello")
    p.FormfeedN(10)

    p.SetAlign("center")
    p.Write("test")
    p.Linefeed()
    p.Write("test")
    p.Linefeed()
    p.Write("test")
    p.FormfeedD(200)

    p.Cut()

    w.Flush()
}
```
