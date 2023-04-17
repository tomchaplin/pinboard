# Pinboard

[![Crates.io - Pinboard](https://img.shields.io/crates/v/pinboard.svg)](https://crates.io/crates/pinboard) [![Build Status](https://travis-ci.org/bossmc/pinboard.svg?branch=master)](https://travis-ci.org/bossmc/pinboard) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

An eventually-consistent, lock-free, mutable store of shared data.

> Just stick it on the pinboard!

## Documentation

https://docs.rs/pinboard/

## Usage

Install from crates.io by adding `pinboard` to your `Cargo.toml`:

```text
[dependencies]
pinboard = "2.0.0"
```

Now you can create a Pinboard, share it between your users (be they `Futures`, threads or really anything else) and start sharing data!

```rust,no_run
use pinboard::Pinboard;
use std::{thread, time::Duration};

let weather_report = Pinboard::new("Sunny");

crossbeam::scope(|scope| {
  scope.spawn(|_| {
    thread::sleep(Duration::from_secs(10));
    weather_report.set("Raining");
  });
  scope.spawn(|_| {
    loop {
      println!("The weather is {:?}", weather_report.get_ref().as_deref());
      thread::sleep(Duration::from_secs(1));
    }
  });
});
```
