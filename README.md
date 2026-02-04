# docker-demo-cpp
A C++ project to test docker
# Dockerized C++ Demo

A simple C++ project containerized with Docker.  
This project demonstrates how to build and run a C++ application inside a Docker container, ensuring consistent builds across different environments.

---

## Features

- C++17 project using `CMake`
- Runs in a **Docker container** with all dependencies included
- Demonstrates **Docker multi-stage build** (optional)
- Portable and reproducible across any system with Docker installed

---
## Using Docker

Docker doesn’t magically speed up compilation (your host CPU still matters), but it guarantees reproducibility, isolates dependencies, simplifies deployment, and makes collaboration/CI/CD painless. For long C++ builds, that can save hours of setup, debugging, and “it works on my machine” headaches.

🔹 Real C++ Example

Imagine a C++ project:

Takes 30 minutes to compile

Uses Boost, OpenCV, and some custom libraries

Without Docker:

You have to install everything manually

On a new machine, compilation might fail due to missing libraries or version mismatch

With Docker:

Your Dockerfile ensures:

Ubuntu 24.04

GCC 13

Boost 1.81

OpenCV 4.x

---
## Project Structure

docker-cpp-demo/
├── Dockerfile # Docker configuration
├── CMakeLists.txt # CMake build file
├── main.cpp # Main C++ source file
└── README.md # This file


---

## Prerequisites

- Ubuntu 24.04 or any system with Docker installed
- Docker version 20+ (tested with Docker 29.2.1)

---

## Build & Run with Docker

1️⃣ Build the Docker image


docker build -t cpp-demo .

2️⃣ Run the application

docker run --rm cpp-demo

Hello from Dockerized C++ app!

## Inspecting the Container

To explore the container filesystem or see the build/ directory:

docker run -it cpp-demo /bin/bash
cd /app
ls -l

Note: Containers run with --rm are deleted after exit.
To keep it alive for inspection, run:

docker run -it --name mycpp cpp-demo /bin/bash

## Optional: Multi-Stage Build

A multi-stage Dockerfile can reduce final image size by only including the compiled binary:

# Stage 1: Build
FROM ubuntu:24.04 AS builder
RUN apt-get update && apt-get install -y build-essential cmake
WORKDIR /app
COPY . .
RUN mkdir build && cd build && cmake .. && make

# Stage 2: Runtime
FROM ubuntu:24.04
WORKDIR /app
COPY --from=builder /app/build/app .
CMD ["./app"]
