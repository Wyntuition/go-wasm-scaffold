# Scaffold: Front End with WebAssembly and Go

Follow the steps to stand up a front end powered by WebAssembly and Go in 15 minutes. A working example is included.

1. Wire up a new go package (main.go hello world included) 

1. Set Go OS and architecture to compile for WebAssembly:

    `export GOOS=js GOARCH=wasm`
    `go build -o main.wasm`

    That will build the package and produce an executable WebAssembly module file named main.wasm. The .wasm file extension will make it easier to serve it over HTTP with the correct Content-Type header later on.

1. To execute main.wasm in a browser, we’ll also need a JavaScript support file, and a HTML page to connect everything together.

    Copy the Go JavaScript support file:

    `cp "$(go env GOROOT)/misc/wasm/wasm_exec.js" .`

1. Creat an `index.html` file:

    ```html
    <html>
        <head>
            <meta charset="utf-8"/>
            <script src="wasm_exec.js"></script>
            <script>
                const go = new Go();
                WebAssembly.instantiateStreaming(fetch("main.wasm"), go.importObject).then((result) => {
                    go.run(result.instance);
                });
            </script>
        </head>
        <body></body>
    </html>
    ```

1. Serve these 3 files:

    ```bash
    # install goexec: go get -u github.com/shurcooL/goexec
    goexec 'http.ListenAndServe(`:8080`, http.FileServer(http.Dir(`.`)))'
    ```

1. Open the JavaScript debug console and navigate to `http://localhost:8080/index.html`, and you should see the output. You can modify the program, rebuild main.wasm, and refresh to see new output.

    You can run your Go functions from the console.

# References

- [Go WebAssembly Docs](https://github.com/golang/go/wiki/WebAssembly)
- [Go WebAssembly Tutorial Video (15m)](https://www.youtube.com/watch?v=4kBvvk2Bzis)

DOM
- [syscall js](ttps://godoc.org/syscall/js)
- [gas web components](https://gascore.github.io/#/)
- [go-app: DOM element access in Go code, progressive web apps](https://github.com/maxence-charriere/go-app)
- [Server-side rendering](https://github.com/bgokden/gowebian)
- [UI library](https://github.com/vugu/vugu)
