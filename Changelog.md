# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

Initial release

### Added

- Run using docker-compose.yml

### Changed

- **Breaking:** Split AC and DC channels into different measurements, you usually do not want both in one query.
  Measurements become:
  - output_per_dtu
  - input_per_dtu
  - output_per_inverter
  - input_per_inverter
  - output
  - input
- Set telegraf interval to `5s`.  

### Fixed

- Correct some typos

