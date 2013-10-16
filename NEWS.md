capwap
======

Erlang CAPWAP AC implementation.

Version 1.1.3 - xx Sep 2013
---------------------------

* Robuster main socket handling, make sure unexpected incomming data
  is unable to crash the AC

Version 1.1.2 - 27 Sep 2013
---------------------------

* handle wtp version decoding errors in discovery gracefully

Version 1.1.1 - 20 Sep 2013
---------------------------

* strict Echo Timeout handling for the control channel
* Data Channel KeepAlive messages are now RFC compliant, invalid
  messages are still accepted
* Send Accounting-Stop on WTP connection loss

Version 1.1.0 - 20 Sep 2013
---------------------------

* Add work arround for pre 1.0 WTP statistics event and handle
  broken (little-endian) encoding
* Add RADIUS accounting for WTP sessions
* Add WTP WWAN statistics in RADIUS accounting
  (requires ctld 1.0.1 and eradius 0.3.1)

Version 1.0.0 - 17 Sep 2013
---------------------------

* Support for local mac and split mac mode
* Allows encryoted and plain WTP control session
* Support for WTP session take over when using encrypted control channel
* Support for station take over