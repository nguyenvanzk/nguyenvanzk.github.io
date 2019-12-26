---
layout: post
title: swift, Modulemap, Static library
date: 2019-12-26 14:59 +0700
---
{% include toc.html %}

# Modulemap
- Dùng module map để tạo framework hoặc static library tự động. (?)
- Tham chiếu đến thư viện system, để dùng trong project.

## Modular framework
```swift
framework module AFramework {
  umbrella header "AFramework.h"

  export *
  module * { export * }
}
```

## Static library
TBD

## Using static library
```swift
module CSDL2 [system] {
    header "shim.h"
    link "SDL2"
    export *
}
```

TBD

# links 
> [http://nsomar.com/modular-framework-creating-and-using-them/]
(http://nsomar.com/modular-framework-creating-and-using-them/)
> [https://clang.llvm.org/docs/Modules.html#id26] 
(https://clang.llvm.org/docs/Modules.html#id26)
> [https://medium.com/allatoneplace/challenges-building-a-swift-framework-d882867c97f9]
(https://medium.com/allatoneplace/challenges-building-a-swift-framework-d882867c97f9)
