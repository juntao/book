---
sidebar_position: 1
---

# 2.1 WasmEdge Features

WasmEdg (project under CNCF) is a lightweight, high-performance, and extensible WebAssembly runtime.

## High Performance

Through [the AoT compiler](/docs/build-and-run/aot.md), WasmEdge is the fastest WebAssembly runtime on the market.

* [A Lightweight Design for High-performance Serverless Computing](https://arxiv.org/abs/2010.07115), published on IEEE Software, Jan 2021. [https://arxiv.org/abs/2010.07115](https://arxiv.org/abs/2010.07115)
* [Performance Analysis for Arm vs. x86 CPUs in the Cloud](https://www.infoq.com/articles/arm-vs-x86-cloud-performance/), published on infoQ.com, Jan 2021. [https://www.infoq.com/articles/arm-vs-x86-cloud-performance/](https://www.infoq.com/articles/arm-vs-x86-cloud-performance/)
* [WasmEdge is the fastest WebAssembly Runtime in Suborbital Reactr test suite](https://blog.suborbital.dev/suborbital-wasmedge), Dec 2021

## Cloud-native Extensions

Besides WASI and the standard WebAssembly proposal, WasmEdge has some cloud-native extensions.

* non-blocking network sockets and web services with Rust, C, and JavaScript SDK
* MySQL-based database driver
* Key value store
* Gas meter for resource limitation
* WasmEdge-bindgen for complex para passing
* AI inference with TensorFlow Lite, Pytorch, and OpenVINO

## JavaScript Support

Through the [WasmEdge-Quickjs](https://github.com/second-state/wasmedge-quickjs) project, WasmEdge could run a JavaScript program, lowering the bar for developing a Wasm app.

* ES6 module and std API support
* NPM module support
* Native JS API in Rust
* Node.js API Support
* Async networking
* Fetch API
* React SSR

## Cloud native orchestration

WasmEdge could be seamlessly integrated with the existing cloud-native infra.

There are several options to manage Wasm apps as "containers" under Kubernetes. All options will give you a Kubernetes cluster that runs Linux containers and WasmWasm containers side by side. 

Option #1 is to [use an OCI runtime crun](/docs/deploy/oci-runtime/crun.md) (the C version of runc — mainly supported by Red Hat). crun decides whether an OCI image is WasmWasm or Linux based on image annotations. If the image is annotated as wasm32, crun will bypass the Linux container setup and just use WasmEdge to run it. Based on crun, we can get the entire Kubernetes stack CRI-O, containerd, Podman, kind, micro k8s, and k8s to work with Wasm images. 

Option #2 is to [use a containerd-shim to start Wasm "containers" via runwasi](/docs/deploy/oci-runtime/containerd.md). Basically, containerd could look at the image's target platform. It uses runwasi if the image is wasm32 and runc if it is x86 / arm. This is the approach behind Docker + Wasm.

## Cross Platform

Wasm is Portable. The compiled wasm file could run on different hardware and platforms without any changes.

WasmEdge supports a wide range of operating systems and hardware platforms. It allows WebAssembly applications to be truly portable across platforms. It runs on Linux-like systems and microkernels such as the `seL4` real-time system.

WasmEdge now supports:

* Linux (x86_64 and aarch64)
* MacOS (x86_64 and M1)
* Windows
* Android
* seL4
* OpenWrt
* OpenHarmony
* Raspberry Pi
* RISC-V (WIP)

## Easy extensible

It is easy to build customized WasmEdge runtime with native host functions in C, Go, and Rust.

Or you could build your own plugins for WasmEdge in C++, C, or Rust (WIP).

## Easy to Embed into a Host Application

Embedded runtime is the classical use case for WasmEdge. You could embed WasmEdge functions in C, Go, Rust, Node.js, Java (WIP), and Python (WIP) host applications.








 