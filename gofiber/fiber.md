# New App
```golang
app:=fiber.New(fiber.Config{
    // 413 bodylimit   /B
    BodyLimit: 50 * 1024 * 1024,
    StrictRouting: true,
})
```
# App add route
```golang
app.Get("/path",handler)
app.Post("/path",handler)
app.Put("/path",handler)
app.Delete("/path",handler)
```
# fiber.Handler
```golang
func FuncName(c *fiber.Ctx)error{
    type Qreq struct{
        ID  uint64 `query:"id" json:"id"`
    }
    var qreq Qreq
    // request raw body
    c.BodyParser(&qreq)
    // request query
    c.QueryParser(&qreq)
    return c.Status(200).JSON(fiber.Map{"status":200,"msg":"hello world","data":qreq})
}
```
# middleware use
```golang
//it will use on all app route url:port
app.Use(fiber.Handler)
//it will use on all app2 route url:port/path
app2 := app.Group("/path")
app2.User(fiber.Handler)
```
## <b><font color=Red>used for middleware must return</font></b>
```golang
return c.Next()
```

# fiber proxy

```golang
// use the following url will trigger proxy
// localhost:port/other/version/env
app.Use("/other",proxyHandler)
func proxyHandler()fiber.Handler{
    prefix := "/version/env"
	return proxy.Balancer(proxy.Config{
		Servers: []string{serverIP},
		ModifyRequest: func(c *fiber.Ctx) error {
			c.Path(strings.TrimPrefix(c.Path(), prefix))
			return c.Next()
		},
		Timeout: 100 * time.Second,
	})
}
```