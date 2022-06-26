# Scheduler

[![test](https://github.com/konrads/pallet-scheduler-datetime/workflows/test/badge.svg)](https://github.com/konrads/pallet-scheduler-datetime/actions/workflows/test.yml)

A module for scheduling dispatches.

- [`scheduler::Config`](https://docs.rs/pallet-scheduler/latest/pallet_scheduler/trait.Config.html)
- [`Call`](https://docs.rs/pallet-scheduler/latest/pallet_scheduler/enum.Call.html)
- [`Module`](https://docs.rs/pallet-scheduler/latest/pallet_scheduler/struct.Module.html)

## Overview

This module exposes capabilities for scheduling dispatches to occur at a
specified schedule based on starting UTC time, optional ending UTC time, and
periods consisting of multiples of year/month/week/day/hour/minute/second/ms.
These scheduled dispatches may be named or anonymous and may be canceled.

Scheduling is done via [chrono-light](https://crates.io/crates/chrono-light) library.

**NOTE:** The scheduled calls will be dispatched with the default filter
for the origin: namely `frame_system::Config::BaseCallFilter` for all origin
except root which will get no filter. And not the filter contained in origin
use to call `fn schedule`.

If a call is scheduled using proxy or whatever mechanism which adds filter,
then those filter will not be used when dispatching the schedule call.

## TODO

- backfill runs in case of downtime
- update libs to latest stable substrate version

## Interface

### Dispatchable Functions

- `sync_scheduleds` - recalculate scheduled wake triggers, accounting for
  potential clock drift.
- `schedule` - schedule a dispatch, which may be periodic, to occur at a
  specified block and with a specified priority.
- `cancel` - cancel a scheduled dispatch, specified by block number and
  index.
- `schedule_named` - augments the `schedule` interface with an additional
  `Vec<u8>` parameter that can be used for identification.
- `cancel_named` - the named complement to the cancel function.

License: Unlicense
