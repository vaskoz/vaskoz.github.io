---
layout: post
title: Go parallelize your tests
---

_As a note to the reader, all my code examples use 'tip' for the Go runtime._
```
go version devel +006bc57095 Sun Oct 22 15:50:50 2017 +0000 darwin/amd64
```

Go's [testing package](https://golang.org/pkg/testing/) has a method to [parallelize the execution of your tests](https://golang.org/pkg/testing/#T.Parallel). Parallelizing your tests and running with the race detector can help detect unexpected shared mutable state.

Below is an example that demonstrates this possible interaction.

{% gist 1fd80dd81e14d87b3a720c0481f6d016 parallel_test.go %}

Below are two different commands that execute tests sequentially or in parallel: "-parallel=1 runs tests sequentially and -parallel=2 runs that in parallel".

{% gist 1fd80dd81e14d87b3a720c0481f6d016 output.txt %}

You should include `t.Parallel()` on the first line of all your unit tests by default. I am not suggesting running parallel tests as the best or only way to find shared mutable tests. My suggestion is merely an additional safeguard. Parallel tests also execute faster by utilizing multiple processors. These benefits behoove me to recommend parallelizing all Go unit tests.