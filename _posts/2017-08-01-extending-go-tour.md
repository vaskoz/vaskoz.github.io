---
---

["A Tour of Go"](https://tour.golang.org/) is a web-based introduction to the [Go programming language](https://golang.org/). The site runs on [Google’s AppEngine platform](https://cloud.google.com/appengine/). However, the Google AppEngine platform has limitations that limit the usefulness of this resource. Alternatively, GoTour can execute from source code which is the focus of this blog post. One can extend the [tool's source code](https://github.com/golang/tour) and content to provide a better learning experience for experienced developers. Extending GoTour in this way can be used as an onboarding tool as new engineers join your organization. It can educate new members of your team on best practices, techniques, and libraries used by your group. In this post, we'll explore how to make the most of the GoTour tool in your organization.

First, let’s review how the hosted [tour.golang.org](https://tour.golang.org/) works. When you go to the first page, you land on the welcome page that has a playground ready to run a "hello world" program. Clicking “Run” sends an encoded form data HTTP POST request that represents the Go source code to run on the site to execute and return output as a response.

Below is an example of a request and a response to
[tour.golang.org](https://tour.golang.org/).
```
curl 'https://tour.golang.org/compile' -H 'origin: https://tour.golang.org' -H 'accept-encoding: gzip, deflate, br' -H 'accept-language: en-US,en;q=0.8' -H 'x-requested-with: XMLHttpRequest' -H 'pragma: no-cache' -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.78 Safari/537.36' -H 'content-type: application/x-www-form-urlencoded; charset=UTF-8' -H 'accept: application/json, text/javascript, */*; q=0.01' -H 'cache-control: no-cache' -H 'authority: tour.golang.org' -H 'referer: https://tour.golang.org/welcome/1' -H 'dnt: 1' --data 'version=2&body=package+main%0A%0Aimport+%22fmt%22%0A%0Afunc+main()+%7B%0A%09fmt.Println(%22Hello%2C+%E4%B8%96%E7%95%8C%22)%0A%7D%0A' --compressed
```
```
{"Errors":"","Events":[{"Message":"Hello, 世界\n","Kind":"stdout","Delay":0}]}
```

Another major limitation of running on the hosted version is the number of threads of execution available is ONE. While you can still launch goroutines and run concurrently, your code cannot run in parallel.

```go
fmt.Println(runtime.NumCPU()) // prints "1" in AppEngine
```

The final limitation is that you cannot use anything but the standard library on the hosted tour.golang.org. You cannot import external packages on the hosted version. All of these limitations plus the fact that you can't customize the content should be convincing enough to switch to a self-hosted and customized version of GoTour.

# How self-hosted is different

First, we need to install GoTour locally. The rest of the instructions assume you have the [Go runtime installed](https://golang.org/doc/install) already.

```go get golang.org/x/tour/gotour``` will build and install. ```$GOPATH/bin/gotour``` will run the program.

Executing code on this version of GoTour no longer sends an HTTP POST request. Now requests and responses travel through a WebSocket.

```
curl 'ws://127.0.0.1:3999/socket' -H 'Pragma: no-cache' -H 'Origin: http://127.0.0.1:3999' -H 'Accept-Encoding: gzip, deflate, br' -H 'Accept-Language: en-US,en;q=0.8' -H 'Sec-WebSocket-Key: n7m3GDPBujHtIz0o6+NACg==' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.78 Safari/537.36' -H 'Upgrade: websocket' -H 'Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits' -H 'Cache-Control: no-cache' -H 'Connection: Upgrade' -H 'Sec-WebSocket-Version: 13' -H 'DNT: 1' --compressed
```

Here is an example of a request:
```javascript
{"Id":"1","Kind":"run","Body":"package main\n\nimport \"fmt\"\nimport \"time\"\n\nfunc main() {\n\ttime.Sleep(10*time.Second)\n\tfmt.Println(\"Hello, 世界\")\n}\n","Options":{"path":"hello.go"}}
```
and the corresponding response:
```javascript
{"Id":"1","Kind":"stdout","Body":"Hello, 世界\n"}
{"Id":"1","Kind":"end","Body":""}
```
The browser's inspector can display the WebSocket activity. I used the Chrome Inspector, specifically the Network tab -> filter by WS "WebSockets".

# My customizations to the tool

First, I wrote some code to enable the race detector. Now to the right of the "Run" button, there is a "RunRace" button. I added content around testing, benchmarking, and data races.

To try this yourself, follow the commands below:
```
go get github.com/vaskoz/extended-tour
```

# Adding content

[This commit](https://github.com/vaskoz/extended-tour/commit/a2418e58b5545dbf2df2af954b5e50ec59a7a72b) illustrates how to add new content.

The procedure begins by adding a new [article file](https://godoc.org/golang.org/x/tools/cmd/present) in the content directory. Then create a subdirectory with the same name.

Finally, edit the "static/js/values.js" file to add the new section.

```
{'id': 'testing', 'title': 'Testing and Benchmarking in Go', 'description': '<p>Testing Go Code</p><p>This module goes over testing, benchmarking and code coverage in Go</p>', 'lessons': ['testing']}
```

# Conclusion

GoTour is a great tool to learn Go interactively. I suggest modifying it to fit your training needs. It can serve as your repository of best practices for your group. [My version](https://github.com/vaskoz/extended-tour) contains features like the race detector and new content that you can modify to fit your specific needs. The reception of this tool for onboarding has been very positive at my organization. Next, I'll be adding examples of libraries that serve specific purposes: circuit-breaking, rate limiting, Kafka and Redis clients, and more. If you self-host, there are no limitations to what packages you can import and run.
