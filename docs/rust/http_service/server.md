---
sidebar_position: 2
---

# 4.3.1 Server

In order for WasmEdge to become a cloud-native runtime for microservices, it needs to support HTTP servers. By its very nature, the HTTP server is always asynchronous. In this chapter, we will cover simple HTTP servers based on the wrap API, as well as low level hyper and socket APIs.

For HTTP clients in WasmEdge, please see [the previous chapter](client.md).

> Before we started, make sure [you have Rust and WasmEdge installed](/docs/rust/setup.md).


## The simple approach

You could use the warp API to create an asynchronous HTTP server. Here are some examples. First, you will need to import the WasmEdge adapted warp crate, which uses a special version of single threaded Tokio that is adapted for WebAssembly.

Work in progress


## The hyper API

The warp crate is convenient to use. But often times, developers need access lower level APIs. The hyper crate is an excellent HTTP library for that. Here are some [examples](https://github.com/WasmEdge/wasmedge_hyper_demo/tree/main/server). First, you will need to import the WasmEdge adapted hyper crate, which uses a special version of single threaded Tokio that is adapted for WebAssembly.

Just add the following line to your Cargo.toml.
```
[dependencies]
hyper_wasi = "0.15.0"
```

The example below shows an HTTP server that echoes back any incoming request.

```
async fn echo(req: Request<Body>) -> Result<Response<Body>, hyper::Error> {
    match (req.method(), req.uri().path()) {
        // Serve some instructions at /
        (&Method::GET, "/") => Ok(Response::new(Body::from(
            "Try POSTing data to /echo such as: `curl localhost:8080/echo -XPOST -d 'hello world'`",
        ))),

        // Simply echo the body back to the client.
        (&Method::POST, "/echo") => Ok(Response::new(req.into_body())),

        (&Method::POST, "/echo/reversed") => {
            let whole_body = hyper::body::to_bytes(req.into_body()).await?;

            let reversed_body = whole_body.iter().rev().cloned().collect::<Vec<u8>>();
            Ok(Response::new(Body::from(reversed_body)))
        }

        // Return the 404 Not Found for other routes.
        _ => {
            let mut not_found = Response::default();
            *not_found.status_mut() = StatusCode::NOT_FOUND;
            Ok(not_found)
        }
    }
}
```

You can build and run [the example](https://github.com/WasmEdge/wasmedge_hyper_demo/blob/main/server/) in WasmEdge as follows.

```
# Git clone or fork the example repo
$ git clone https://github.com/WasmEdge/wasmedge_hyper_demo.git
$ cd wasmedge_hyper_demo/server

# Build the Rust code
cargo build --target wasm32-wasi --release

# Use the AoT compiler to get better performance
wasmedgec target/wasm32-wasi/release/wasmedge_hyper_server.wasm target/wasm32-wasi/release/wasmedge_hyper_server.wasm

# Run the example
wasmedge target/wasm32-wasi/release/wasmedge_hyper_server.wasm
```






