# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## 6.10.0 - 2019-04-03

Starting from 6.10.0, all of these Kurento Media Server sub-projects

- kurento-module-creator
- kms-cmake-utils
- kms-core
- kms-elements
- kms-filters
- kms-jsonrpc
- kurento-media-server

have their ChangeLogs unified into this one.

### Added

- Support for Ubuntu 18.04. All build scripts and Debian packaging files were reviewed.
- CMake helper to load Sanitizers (ASan, TSan, etc). This integrates https://github.com/arsenm/sanitizers-cmake
- A new 'timestampMillis' field in RPC *Event* and *Stats* messages indicates the message timestamp with more precission than the old 'timestamp', which is now **deprecated**.
- Support for MKV multimedia container has been added to the RecorderEndpoint, using H.264 for the video encoding. Thanks [@prlanzarin](https://github.com/prlanzarin) (Paulo Lanzarin) for [Kurento/kms-core#14](https://github.com/Kurento/kms-core/pull/14), [Kurento/kms-elements#13](https://github.com/Kurento/kms-elements/pull/13).
- Improved the [Troubleshooting Issues](https://doc-kurento.readthedocs.io/en/6.10.0/user/troubleshooting.html) section with new cases. Thanks [@tioperez](https://github.com/tioperez) (Luis Alfredo Perez Medina) for reporting [#349](https://github.com/Kurento/bugtracker/issues/349) and sharing his results with RTSP and Docker.
- Added support for AARCH64 architecture in the Death Handler library. Thanks [@goroya](https://github.com/goroya) for [Kurento/kurento-media-server#10](https://github.com/Kurento/kurento-media-server/pull/10).

### Changed

- Use `-std=gnu11` and `-std=gnu++11` to enable C and C++ language versions with GNU extensions.
- Use default build modes from CMake, only adding `-Werror -O0` in case of *Debug* builds.
- Rewrite the JSON parsing of settings files, to allow "inline comments" inside the JSON field names. Starting from Ubuntu 18.04, the underlying support library (Boost) doesn't accept comments in JSON any more; an alternative has been devised to overcome this limitation. Instead of commenting lines like before, it's now possible to write the comment marker "//" *inside* the field names. For example: `"//name": "H264"`.
- Clearer transcoding debug messages in the agnostic component (`KmsAgnosticBin`). Check out the [6.10 Release Notes](https://doc-kurento.readthedocs.io/en/6.10.0/project/relnotes/v6_10_0.html) for a detailed description of what changed.

### Fixed

- The GStreamer payloaders in `KmsBaseRtpEndpoint` and `KmsRtpPayTreeBin` were checking for the wrong data type before setting the `config-interval` property. Thanks [@leetal](https://github.com/leetal) (Alexander Widerberg) for reporting [#321](https://github.com/Kurento/bugtracker/issues/321) and [@prlanzarin](https://github.com/prlanzarin) (Paulo Lanzarin) for [Kurento/kms-core#15](https://github.com/Kurento/kms-core/pull/15).
- Avoid setting NULL to `port-range` property in `uridecodebin` element when playing RTSP streams. This caused a segmentation fault due to a bug in GStreamer. Filed a patch in [gstreamer/gst-plugins-good!64](https://gitlab.freedesktop.org/gstreamer/gst-plugins-good/merge_requests/64). Thanks [@823639792](https://github.com/823639792) for reporting [#325](https://github.com/Kurento/bugtracker/issues/325).
- Catch proper exceptions on parse error for settings files. Before, parsing errors would pass silently.


## [6.8.1] - 2018-10-23

### Fixed
- Service init files will now append to the error log file, instead of truncating it on each restart.

## [6.8.0] - 2018-09-26

### Added
- GLib logging messages are now integrated with Kurento logging. This means that it's possible to save debug messages from libnice, together with the usual logs from Kurento / GStreamer.

### Changed
- Output logs now use standard format ISO 8601.
- `disableRequestCache` is now exposed in settings file (*kurento.conf.json*). This can be used to disable the RPC Request Cache.
- Clearer log messages about what is going on when the maximum resource usage threshold is reached.
- File `/etc/default/kurento-media-server` now contains more useful examples and explanations for each option.

## [6.7.2] - 2018-05-11

### Fixed
- All: Apply multiple fixes suggested by *clang-tidy*.
- Re-add redirection of 'stderr': log DeathHandler messages.
- [#245](https://github.com/Kurento/bugtracker/issues/245) (Possible SYN flooding): WebSocketTransport: Change default listen backlog to `socket_base::max_connections`.
- [#242](https://github.com/Kurento/bugtracker/issues/242) (libSSL crashes on mirrored packets): Debian: Remove dependency on our unmaintained fork of libSSL - **Work In Progress**.
- Debian: Remove unneeded build dependency: binutils.
- Debian: Use better defaults for logging levels.

## [6.7.1] - 2018-03-21

### Changed
- Raise version to 6.7.1.

## [6.7.0] - 2018-01-24

### Changed
- CMake: Compile and link as Position Independent Executable ('-fPIE -pie').
- Add more verbose logging in some areas that required it.
- Debian: Align all version numbers of KMS-related modules.
- Debian: Remove version numbers from package names.
- Debian: Configure builds to use parallel compilation jobs.

### Fixed

- Remove usage of 'sudo' from init script.

## [6.6.2] - 2017-07-24

### Changed
- Old ChangeLog.md moved to the new format in this CHANGELOG.md file.
- CMake: Full review of all CMakeLists.txt files to tidy up and homogenize code style and compiler flags.
- CMake: Position Independent Code flags ("-fPIC") were scattered around projects, and are now removed. Instead, the more CMake-idiomatic variable "CMAKE_POSITION_INDEPENDENT_CODE" is used.
- CMake: All projects now compile with "[-std=c11|-std=c++11] -Wall -Werror -pthread".
- CMake: Debug builds now compile with "-g -O0" (while default CMake used "-O1" for Debug builds).
- CMake: include() and import() commands were moved to the code areas where they are actually required.

### Fixed
- Fix missing header in "server/loadConfig.cpp".

## [6.6.1] - 2016-09-30

### Changed
- Fixes on tests.
- Improve compilation process.

## [6.6.0] - 2016-09-09

### Fixed
- Minor compilation warnings.
- Fix resource limits checking; if a limit is not configured then the check wasn't being done.

## [6.5.0] - 2016-05-27

### Changed
- Changed license to Apache 2.0.
- Updated documentation.
- Improve performance of RPC proccessing.
- Allow qualified names for types.

### Fixed
- Bug on client reconnection (they thought that the reconnection succeed even if it was a diferent server).

## [6.4.0] - 2016-02-24

### Fixed
- Update websocketpp library to version 0.7.0. This fixes segmentation fault with wss and more than one thread.

## [6.3.3] - 2016-02-01

### Fixed
- Installation script.

## [6.3.2] - 2016-01-29

### Fixed
- Problem with write permissions to log folder.
- WebsocketTransport: Fix bug on session injection when there are no parameters.

## [6.3.1] - 2016-01-20

### Changed
- Create a kurento user to allow buffering of played medias.

## [6.3.0] - 2019-01-19

### Added
- Print compilation time and date on log for debugging purposes.
- Print stack trace when abort or segfault signals are captured.
- Add "closeSession" method to release/dispose all session resources.

### Removed
- Support for RabbitMQ.

### Fixed
- Fix memory leaks in websockettransport.
- Fix session management in websocket (just one session was used).

## 6.2.0 - 2015-11-25

### Added
- New Ping/Pong based protocol for keeping connections alive.

### Fixed
- Scaffold: Fix installation of configuration files.

[6.8.1]: https://github.com/Kurento/kurento-media-server/compare/6.8.0...6.8.1
[6.8.0]: https://github.com/Kurento/kurento-media-server/compare/6.7.2...6.8.0
[6.7.2]: https://github.com/Kurento/kurento-media-server/compare/6.7.1...6.7.2
[6.7.1]: https://github.com/Kurento/kurento-media-server/compare/6.7.0...6.7.1
[6.7.0]: https://github.com/Kurento/kurento-media-server/compare/6.6.2...6.7.0
[6.6.2]: https://github.com/Kurento/kurento-media-server/compare/6.6.1...6.6.2
[6.6.1]: https://github.com/Kurento/kurento-media-server/compare/6.6.0...6.6.1
[6.6.0]: https://github.com/Kurento/kurento-media-server/compare/6.5.0...6.6.0
[6.5.0]: https://github.com/Kurento/kurento-media-server/compare/6.4.0...6.5.0
[6.4.0]: https://github.com/Kurento/kurento-media-server/compare/6.3.3...6.4.0
[6.3.3]: https://github.com/Kurento/kurento-media-server/compare/6.3.2...6.3.3
[6.3.2]: https://github.com/Kurento/kurento-media-server/compare/6.3.1...6.3.2
[6.3.1]: https://github.com/Kurento/kurento-media-server/compare/6.3.0...6.3.1
[6.3.0]: https://github.com/Kurento/kurento-media-server/compare/6.2.0...6.3.0
