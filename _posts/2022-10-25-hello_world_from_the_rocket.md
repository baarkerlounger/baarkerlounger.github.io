---
layout: post
title: Hello world from the rocket
subtitle: A full stack web app in Rust - Part 2
gh-repo: https://github.com/baarkerlounger/govuk-prototype-rs
tags: [rust, rocket, web dev]
comments: true
---

Having chosen Rocket the first thing to note is that the last released version (0.4.11) is effectively in maintenance mode with a 0.5 release currently at release candidate stage and bringing some pretty big changes including stable async support throughout and a few library upgrades and deprecations. The 0.5 release has been a little slow going as there's a foundation being set up for the framework at the same time but it also seems relatively stable at this point (including upgrade docs etc) so given this is a brand new app it made sense to start straight off with the release candidate.

With that disclaimer out of the way it's onto the initial setup. This is pretty well covered by the Rocket guide docs and involved fairly minimal boilerplate. After creating our new cargo project, we add a single dependency:

```rust
# Cargo.toml

[package]
name = "govuk-prototype-rs"
version = "0.1.0"
edition = "2021"

[dependencies]
rocket = "0.5.0-rc.2"

```
a single route and handler, and a launch (main) function that mounts our route. The launch "function" is really an attribute macro that generates our main function and starts our web server.

```rust
# src/main.rs

#[macro_use]
extern crate rocket;

#[get("/")]
fn start_page() -> &'static str {
    "Hello, world!"
}

#[launch]
fn rocket() -> _ {
    rocket::build().mount("/", routes![start_page])
}

#[cfg(test)]
mod test {
    use super::rocket;
    use rocket::http::Status;
    use rocket::local::blocking::Client;

    #[test]
    fn start_page() {
        let client = Client::tracked(rocket()).unwrap();
        let req = client.get("/");
        let response = req.dispatch();
        let expected_content = "Hello, world!";
        assert_eq!(response.status(), Status::Ok);
        assert_eq!(
            response.into_string().unwrap().contains(expected_content),
            true
        );
    }
}
```

We can run our test with `cargo test`. Rocket provides some test helpers, like the blocking client used above, to make it easier to write tests for the router/handler system.

We can also have cargo generate the directory structure and dependency manifest for us:

```bash
cargo new <my-project>
cargo add rocket
```

<br/>
### So far, so good
Quick and easy to get up and running, and straightforward to TDD as well.
<br/><br/>
