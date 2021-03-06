# Actix-web

`Async-graphql-actix-web` provides an implementation of `actix_web::FromRequest` for `Request`.
This is actually an abstraction around `async_graphql::Request` and you can call `Request::into_inner` to 
convert it into a `async_graphql::Request`.

`WSSubscription` is an Actor that supports WebSocket subscriptions.

## Request example

When you define your `actix_web::App` you need to pass in the Schema as data. 

```rust
async fn index(
    // Schema now accessible here
    schema: web::Data<Schema>,
    request: async_graphql_actix_web::Request,
) -> web::Json<Response> {
    web::Json(Response(schema.execute(request.into_inner()).await)
}

```

## Subscription example

```rust
async fn index_ws(
    schema: web::Data<Schema>,
    req: HttpRequest,
    payload: web::Payload,
) -> Result<HttpResponse> {
    WSSubscription::start(Schema::clone(&*schema), &req, payload)
}
```

## More examples

[https://github.com/async-graphql/examples/tree/master/actix-web](https://github.com/async-graphql/examples/tree/master/actix-web)
