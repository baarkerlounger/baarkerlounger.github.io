---
layout: post
title: Off the rails and into the rusty rocket
subtitle: A full stack web app in Rust - Part 1
gh-repo: https://github.com/baarkerlounger/govuk-prototype-rs
tags: [rust, rocket, web dev]
comments: true
---

Having spent the last year or so building digital services for government using Ruby on Rails (and a good chunk of time working on Rails app before that), I thought I'd approach learning Rust by seeing how easily I could put together a similar sort of web application, trying to understand the ecosystem and the current state of web dev in Rust.

The first thing to do it seemed was pick a web server framework. Rust has a few options with similar maturity and mind share, though none of them are really "batteries included" in the way that Rails is, Sinatra/Flask would probably be a more apt comparison. Some fairly cursory research (I was keen to get started with _something_) suggested the biggest 3 options were [Actix-Web](https://actix.rs/), [Rocket](https://rocket.rs/) and [Axum](https://github.com/tokio-rs/axum). Of those, it seemed that Rocket and Axum had somewhat nicer syntax/ergonomics (to the extent that I was able to judge that with my limited Rust knowledge), though both seemed to be in the middle of breaking changes still at release candidate change.

Recently Axum seems a little more actively developed than Rocket and with a bigger team behind it, but Rocket had far more documentation available, including a number of blogs, examples, books and a matrix channel, and had the biggest real world use case I could find in [Vaultwarden.rs](https://github.com/dani-garcia/vaultwarden), an alternative Bitwarden server implementation. In the end I chose to start with Rocket.

From here I'll be trying to recreate a dummy/protoype app that demos the types of functionality I'd usually be expecting to spin up:

- A view layer, initially I'll be looking for server side templating though I might explore client side Rust via WASM later on too
- Integrating static assets from an NPM package (the GovUK Design System)
- A persistence layer, mostly likely a SQL database
- Integrating an external service like Gov Notify
- Offloading tasks to background jobs
- Containerising the app
- Deploying the app somewhere

With that to do list out of the way, time to write GET `/hello-world`.
<br/><br/>
