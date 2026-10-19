##############
Changelog
##############

v 0.1.0 - 2026-10-19
====================
Updated for the current Time Tagger software (tested against 2.22.6).
This release requires **Time Tagger 2.20 or newer** and Python 3.8 or newer.

* The library is now imported from the ``Swabian.TimeTagger`` namespace that
  was introduced in Time Tagger 2.20. Install it on the server machine with
  ``pip install TimeTaggerRPC[server]``, which pulls in the
  ``Swabian-TimeTagger`` package.
* Fixed a server startup crash with Time Tagger 2.18 and newer. The
  ``TimeTagger`` class was split into ``TimeTaggerSource`` and
  ``TimeTaggerHardware``, and these, like ``TimeTaggerServer``, have no
  ``create*()`` factory function. Adapter generation no longer assumes that
  every :class:`ttdoc:TimeTaggerBase` subclass is constructible.
* Fixed ``getDataObject()`` for measurements whose data object does not follow
  the ``<Name>Data`` naming convention. ``HistogramCustomBins`` returns a
  ``HistogramLogBinsData``, and ``PulsePerSecondMonitor`` returns a
  ``PulsePerSecondData``. The data object class is now taken from the return
  type annotation where available, so measurements added in future library
  versions resolve automatically, with a lookup table and the naming
  convention as fallbacks. If no matching class is found, the method is
  skipped with a warning rather than aborting server startup.
* The server logs the Time Tagger library version on startup and warns when
  it is older than the supported minimum.
* Excluded ``FlimBase`` from the exposed members, alongside the already
  excluded ``FlimAbstract``.
* Dropped the compatibility shim for Time Tagger versions that did not use
  the ``enum`` package for ``Resolution``, ``CoincidenceTimestamp`` and
  ``ChannelEdge``.
* Modernized packaging to PEP 621 metadata and raised ``numpy`` to >= 1.23,
  matching the requirement of the Time Tagger library and supporting NumPy 2.
  Two extras keep the core install small: ``[server]`` adds the Time Tagger
  library and ``[examples]`` adds matplotlib for the example scripts.
* Repaired continuous integration, which pinned a retired runner image and
  Python versions. It now also generates every RPC adapter against the real
  Time Tagger library on Linux, which requires no hardware.

Newer measurement classes and virtual channels are picked up automatically by
the server's introspection and need no changes here. This covers
``FrequencyCounter``, ``PulsePerSecondMonitor``, ``PhaseNoise``,
``CorrelationPairs``, ``HistogramCustomBins``, ``GatedCounter``,
``Combinations`` and ``GatedChannels``.

The Virtual Time Tagger remains unsupported over RPC. Its 2.18 API expects
local files and a ``run()`` call on the machine running the server.


v 0.0.6 - 2022-02-15
====================
* Fixed improper cleanup on client disconnection that resulted in growing server memory and CPU usage.


v 0.0.5 - 2022-01-03
====================
* Added support for iterator methods :meth:`ttdoc:Counter.getDataObject()`.
* Implemented support for :meth:`ttdoc:SynchronizedMeasurements.getTagger()`.
* Improved logging.
* Renamed root Pyro object `TimeTagger` to `TimeTaggerRPC`.
* Typo corrections in the documentation.
* Added this changelog.
