# router

[![Build Status](https://dl.circleci.com/status-badge/img/gh/sourceweaver/router/tree/master.svg?style=shield)](https://dl.circleci.com/status-badge/redirect/gh/sourceweaver/router/tree/master)

## Installation

Add this to your application's `shard.yml`:

```yaml
dependencies:
  router:
    github: sourceweaver/router
```

## Usage

### Basic usage

```crystal
require "router"
```

Include `Router` to utilize **router.cr**.
```crystal
class WebServer
  include Router
end
```

Define a method to draw all routes for your web server.
```crystal
class WebServer
  include Router
  
  def draw_routes
    # Drawing routes HERE!
  end
end
```

In that method, call HTTP method name (downcase) like `get` or `post` with PATH and BLOCK where
 - PATH  : String
 - BLOCK : block of HTTP::Server::Context, Hash(String, String) -> HTTP::Server::Context
```crystal
class WebServer
  include Router

  def draw_routes
    get "/" do |context, params|
      context.response.print "Hello router.cr!"
      context
    end
  end
end
```

Here we've defined a GET route at root path (/) that just print out "Hello router.cr" when we get access.
To activate (run) the route, just define run methods for your server with route_handler
```crystal
class WebServer
  include Router
  
  def draw_routes
    get "/" do |context, params|
      context.response.print "Hello router.cr!"
      context
    end
  end
  
  def run
    server = HTTP::Server.new(route_handler)
    server.bind_tcp 8080
    server.listen
  end
end
```
Here route_handler is getter defined in Router. So you can call `route_handler` at anywhere in WebServer instance.

Finally, run your server.
```crystal
web_server = WebServer.new
web_server.draw_routes
web_server.run
```

See [samples](https://github.com/sourceweaver/router/blob/master/samples/sample.cr) and [tips]([samples](https://github.com/sourceweaver/router/blob/master/samples/tips.cr)) for details.

### Path parameters

`params` is a Hash(String, String) that is used when you define a path parameters such as `/user/:id` (`:id` is a parameters). Here is an example.
```crystal
class WebServer
  include Router

  def draw_routes
    get "/user/:id" do |context, params|
      context.response.print params["id"] # get :id in url from params
      context
    end
  end
end
```

See [samples](https://github.com/sourceweaver/router/blob/master/samples/sample.cr) and [tips]([samples](https://github.com/sourceweaver/router/blob/master/samples/tips.cr)) for details.


## Contributors

- [tbrand](https://github.com/tbrand) Taichiro Suzuki - creator, maintainer
