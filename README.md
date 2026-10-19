[![PyPI Version](https://img.shields.io/pypi/v/TimeTaggerRPC.svg "PyPi Version")](https://pypi.org/project/TimeTaggerRPC/)
[![PyPI - Downloads](https://img.shields.io/pypi/dm/TimeTaggerRPC "PyPI Downloads")](https://pypi.org/project/TimeTaggerRPC/)



Time Tagger RPC implementation using [Pyro5](https://pypi.org/project/Pyro5/).


### About this fork
This is a fork of [swabianinstruments/TimeTaggerRPC](https://github.com/swabianinstruments/TimeTaggerRPC),
which has not been updated since January 2024 and targets a Time Tagger release
older than 2.18. This fork tracks the current Time Tagger software, tested
against **2.22.6**.

The badges above refer to the upstream package on PyPI. This fork is not
published there, so install it from the repository:

```
> pip install "TimeTaggerRPC[server] @ git+https://github.com/fabio-terranova/TimeTaggerRPC"
```


### Alpha version !
This project is in the alpha stage of the development. This means that the code 
successfully passed basic testing and is operational. 
However, some things might be broken, and the API may change in the future versions.


### Requirements

* Python 3.8 or newer
* [Time Tagger software](https://www.swabianinstruments.com/time-tagger/downloads/) 2.20 or newer,
  on the machine with the device attached

Time Tagger 2.20 moved the Python bindings to the `Swabian.TimeTagger` namespace
and publishes them as the [`Swabian-TimeTagger`](https://pypi.org/project/Swabian-TimeTagger/)
package. Wheels are available for Linux and Windows.

### Install

On the machine with the Time Tagger connected, install the server extra to pull
in the hardware library:

```
> pip install TimeTaggerRPC[server]
```

Clients only talk Pyro5 and do not need the hardware library:

```
> pip install TimeTaggerRPC
```

The scripts in `examples/` plot their results, so they also need matplotlib:

```
> pip install TimeTaggerRPC[examples]
```

### Run server
Start the server on a PC with the Time Tagger connected.

```
> TimeTaggerRPC-server --help
usage: TimeTaggerRPC-server [-h] [--host localhost] [--port 23000] [--use_ns] [--start_ns]

--------------------------------------------
Swabian Instruments Time Tagger RPC Server.
--------------------------------------------

optional arguments:
  -h, --help        show this help message and exit
  --host localhost  Hostname or IP on which the server will listen for connections.
  --port 23000      Server port.
  --use_ns          Use Pyro5 name server.
  --start_ns        Start Pyro5 name server in a subprocess.
```


### Client example
Control Time Tagger remotely over the network.


```python
import matplotlib.pyplot as plt
from TimeTaggerRPC import client

with client.createProxy(host='localhost', port=23000) as TT:
   tagger = TT.createTimeTagger()
   tagger.setTestSignal(1, True)
   tagger.setTestSignal(2, True)

   hist = TT.Correlation(tagger, 1, 2, binwidth=5, n_bins=2000)
   hist.startFor(int(10e12), clear=True)

   x = hist.getIndex()
   while hist.isRunning():
      plt.pause(0.1)
      y = hist.getData()
      plt.cla()
      plt.plot(x, y)

   TT.freeTimeTagger(tagger)
   
```

You can find more information about the Time Tagger API in the official
[documentation](https://www.swabianinstruments.com/static/documentation/TimeTagger/index.html).