---
title: Pushing logs with a SDK - Rust - gelf_logger and log4rs-gelf
slug: logs-data-platform-rust-libs
order: 30
section: Logging libraries
routes:
    canonical: 'https://help.ovhcloud.com/csm/en-gb-logs-data-platform-rust-libs?id=kb_article_view&sysparm_article=KB0050066'
updated: 2023-01-16
---

**Last updated 16th January, 2023**

## Objective

This guide will show you how to push your logs to Logs Data Platform using Rust.

Rust has a logging implementation ([log](https://docs.rs/log/*/log/){.external}) which is widely used. OVHcloud has implemented this system to support the [GELF format](https://go2docs.graylog.org/4-x/getting_in_log_data/gelf.html?tocpath=Getting%20in%20Log%20Data%7CLog%20Sources%7CGELF%7C_____0#GELFPayloadSpecification#gelf-payload-specification){.external}:

- **gelf_logger**: This is a minimal logger.
- **log4rs-gelf**: Based on _gelf_logger_, this implementation is compatible with the complex configurable framework [log4rs](https://docs.rs/log4rs/*/log4rs/){.external}.

Those loggers will:

- serialize log entries using the serde_gelf crate.
- bufferize the result into memory.
- batch send over network using TCP/TLS.
- a facility to ensure fields suits the [LDP naming conventions](/pages/platform/logs-data-platform/getting_started_field_naming_convention).

## Requirements

To complete this guide you will need:

- Rust, we recommend the Nightly version.
- [Activated your Logs Data Platform account.](https://www.ovh.com/fr/order/express/#/new/express/resume?products=~%28~%28planCode~%27logs-account~productId~%27logs%29){.external}
- [To create at least one Stream and get its token.](/pages/platform/logs-data-platform/getting_started_quick_start)

## gelf_logger

You can start using it by first adding it to your `Cargo.toml`:

```toml
[dependencies]
gelf_logger = { version = "0.1", features = ["ovh-ldp"] }
# or
[dependencies.gelf_logger]
version = "0.1"
features = ["ovh-ldp"]
```

End then in you `main.rs`:

```rust hl_lines="6"
extern crate gelf_logger;
#[macro_use]
extern crate log;

fn main() {
    let cfg = Config::ldp("gra1.logs.ovh.com", "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX");

    // Initialize logger
    gelf_logger::init(cfg).unwrap();

    // Send log using a macro defined in the create log
    info!("common message");

    // make sure all buffered records are sent before exiting
    gelf_logger::flush().unwrap();
}
```

You could also look at the [generated API documentaton](https://docs.rs/gelf_logger/*){.external}.

## log4rs-gelf

You can start using it by first adding it to your `Cargo.toml`:

```toml
[dependencies]
log4rs_gelf = { version = "0.1", features = ["ovh-ldp"] }
# or
[dependencies.log4rs_gelf]
version = "0.1"
features = ["ovh-ldp"]
```

### Examples

#### From a YAML configuration file

```yaml
appenders:
  ldp:
    additional_fields:
      component: rust-cs
    buffer_duration: 5
    buffer_size: 5
    hostname: 127.0.0.1
    kind: buffer
    level: Informational
    null_character: true
    port: 12202
    use_tls: false
root:
  appenders:
  - ldp
  level: info
```

And then:

```rust
extern crate log4rs_gelf;

fn main() {
    log4rs_gelf::init_file("/tmp/log4rs.yml", None).unwrap();

    // Do whatever

    log4rs_gelf::flush().expect("Failed to send buffer, log records can be lost !");
}
```

#### Programmatically constructing a configuration

```rust hl_lines="9"
extern crate log;
extern crate log4rs;
extern crate log4rs_gelf;

use log4rs::config::{Config, Appender, Root};
use log::LevelFilter;

fn main() {
    let buffer = log4rs_gelf::BufferAppender::builder("gra1.logs.ovh.com","XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX")
        .build()
        .unwrap();

   let config = Config::builder()
       .appender(Appender::builder().build("gelf", Box::new(buffer)))
       .build(Root::builder().appender("gelf").build(LevelFilter::Info))
       .unwrap();

   log4rs_gelf::init_config(config).unwrap();

   // Do whatever

   log4rs_gelf::flush().expect("Failed to send buffer, log records can be lost !");
}
```

You could also look at the [generated API documentation](https://docs.rs/log4rs-gelf/*){.external}.

## Go further

- Getting Started: [Quick Start](/pages/platform/logs-data-platform/getting_started_quick_start)
- Documentation: [Guides](https://docs.ovh.com/fr/logs-data-platform/)
- Community hub: [https://community.ovh.com](https://community.ovh.com/en/c/Platform/data-platforms){.external}
- Create an account: [Try it!](https://www.ovh.com/fr/order/express/#/express/review?products=~(~(planCode~'logs-account~productId~'logs))){.external}
