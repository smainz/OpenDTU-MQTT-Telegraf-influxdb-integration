# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.0] - unrelease

### Added

- Documented how `yieldday` works and why it may not be a good idea to use it.

### Fixed

- AC total was not displayed correctly #5

## [1.1.0] - 2023-06-22

### Added

- Make buckets for OpenDTU data and weather data configurable.

### Fixed

- Use `output_per_dtu instead` measurement instead of `ìnput_per_dtu` to calculate *Aktuelle Leistung AC*.
- Correct calculation of total daily production *Letzte 14 Tage*.

## [1.0.0] - 2023-06-15

Initial release comprising the telegraf config and a single grafana dashboard.
