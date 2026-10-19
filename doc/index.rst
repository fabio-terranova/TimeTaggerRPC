.. Time Tagger RPC documentation master file, created by
   sphinx-quickstart on Thu Apr  1 11:53:59 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

#################################
Remote Time Tagger with Python
#################################

.. image:: /img/timetagger-pyro5-banner.*
   :align: center
   :width: 100 %


Install on the PC with the Time Tagger connected. The ``server`` extra pulls in
the Time Tagger library (2.20 or newer) from PyPi.

::

   > pip install TimeTaggerRPC[server]


Clients communicate over Pyro5 and do not need the Time Tagger library.

::

   > pip install TimeTaggerRPC



Start the server on a PC with the Time Tagger connected.

::

   > TimeTaggerRPC-server


Control Time Tagger remotely over the network.

.. code-block:: python

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


.. toctree::
   :maxdepth: 2
   :caption: Contents:

   cookbook
   internals
   changelog



.. Indices and tables
.. ==================

.. * :ref:`genindex`
.. * :ref:`modindex`
.. * :ref:`search`
