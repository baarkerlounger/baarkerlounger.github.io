---
layout: post
title: Database persistence, filling the rocket with diesel fuel
subtitle: A full stack web app in Rust - Part 5
gh-repo: https://github.com/baarkerlounger/govuk-prototype-rs
tags: [rust, rocket, web dev]
comments: true
---

There's many, many ways you could go here so to narrow the scope a little bit I wanted to add a PostgreSQL database to my application. Ideally with support for:

- Schema migrations as code
- An ORM of some sort
- A mechanism for seeding data
- A mechanism for integration testing the application without needing to mock application code

Rocket doesn't have a built in default ORM like ActiveRecord but it does provide a crate for accessing and managing database connection pools and you can bring your own ORM. There's a async and a blocking version depending on whether you want to use an async or blocking ORM.

<br/>

## Dependency conflicts

The most popular and seemingly best documented ORM I came across in Rust was [Diesel](https://diesel.rs/) so I started with that. This is where I ran into the first fiddly issue, Diesel's latest version (2.0) doesn't work with Rocket's latest version (0.5.0-rc.2), but support has been merged upstream already. Living on the bleeding edge then means or `Cargo.toml` file becomes:

```toml
rocket = { git = "https://github.com/SergioBenitez/Rocket.git", branch = "master", features = ["json"] }
rocket_dyn_templates = { git = "https://github.com/SergioBenitez/Rocket.git", branch = "master", features = ["tera"] }
rocket_sync_db_pools = { git = "https://github.com/SergioBenitez/Rocket.git", branch = "master", features = ["diesel_postgres_pool"] }
diesel = { version = "2.0", features = ["postgres"] }
diesel_migrations = "2.0"
```
<br/>

## Database setup
<br/>

## Migrations
<br/>

## Implement CRUD for a model

<br/>
