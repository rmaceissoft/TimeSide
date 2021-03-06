==================================================
TimeSide : audio processing framework for the web
==================================================

|version| |downloads| |travis_master| |coveralls_master|

.. |travis_master| image:: https://secure.travis-ci.org/Parisson/TimeSide.png?branch=master
    :target: https://travis-ci.org/Parisson/TimeSide/

.. |coveralls_master| image:: https://coveralls.io/repos/Parisson/TimeSide/badge.png?branch=master
  :target: https://coveralls.io/r/Parisson/TimeSide?branch=master

.. |version| image:: https://img.shields.io/pypi/v/timeside.svg
   :target: https://pypi.python.org/pypi/TimeSide/
   :alt: Version

.. |downloads| image:: https://img.shields.io/pypi/dm/timeside.svg
   :target: https://pypi.python.org/pypi/TimeSide/
   :alt: Downloads


TimeSide is a python framework enabling low and high level audio analysis, imaging, transcoding, streaming and labelling. Its high-level API is designed to enable complex processing on very large datasets of any audio or video assets with a plug-in architecture, a secure scalable backend and an extensible dynamic web frontend.


Use cases
==========

* Scaled audio computing (filtering, machine learning, etc)
* Web audio visualization
* Plugin prototyping
* Pseudo-realtime transcoding and streaming
* Automatic segmentation and labelling synchronized with audio events


Goals
=====

* **Do** asynchronous and fast audio processing with Python,
* **Decode** audio frames from **any** audio or video media format into numpy arrays,
* **Analyze** audio content with some state-of-the-art audio feature extraction libraries like Aubio, Yaafe and VAMP as well as some pure python processors
* **Visualize** sounds with various fancy waveforms, spectrograms and other cool graphers,
* **Transcode** audio data in various media formats and stream them through web apps,
* **Serialize** feature analysis data through various portable formats,
* **Playback** and **interact** **on demand** through a smart high-level HTML5 extensible player,
* **Index**, **tag** and **annotate** audio archives with semantic metadata (see `Telemeta <http://telemeta.org>`__ which embed TimeSide).
* **Deploy** and **scale** your own audio processing engine through any infrastructure


Funding and support
===================

To fund the project and continue our fast development process, we need your explicit support. So if you use TimeSide in production or even in development, please let us know:

* star or fork the project on `GitHub <https://github.com/Parisson/TimeSide>`_
* tweet something to `@parisson_studio <https://twitter.com/parisson_studio>`_ or `@yomguy <https://twitter.com/omguy>`_
* drop us an email <support@parisson.com>

Thanks for your help!

Architecture
============

The streaming architecture of TimeSide relies on 2 main parts: a processing engine including various plugin processors written in pure Python and a user interface providing some web based visualization and playback tools in pure HTML5.

.. image:: http://vcs.parisson.com/gitweb/?p=timeside.git;a=blob_plain;f=doc/images/TimeSide_pipe.svg;hb=refs/heads/dev
  :width: 800 px

Dive in
========

Let's produce a really simple audio analysis of an audio file.
First, list all available plugins:

.. doctest::

>>> import timeside.core
>>> timeside.core.list_processors()

Define some processors:

.. doctest::

>>> from timeside.core import get_processor
>>> from timeside.core.tools.test_samples import samples
>>> wavfile = samples['sweep.wav']
>>> decoder  =  get_processor('file_decoder')(wavfile)
>>> grapher  =  get_processor('waveform_simple')()
>>> analyzer =  get_processor('level')()
>>> encoder  =  get_processor('vorbis_encoder')('sweep.ogg')

Then run the *magic* pipeline:

.. doctest::

>>> (decoder | grapher | analyzer | encoder).run()

Render the grapher results:

.. doctest::

>>> grapher.render(output='waveform.png')

Show the analyzer results:

.. doctest::

>>> print 'Level:', analyzer.results  # doctest: +ELLIPSIS
    Level: {'level.max': AnalyzerResult(...)}


So, in only one pass, the audio file has been decoded, analyzed, graphed and transcoded.

For more extensive examples, please see the `full documentation <http://files.parisson.com/timeside/doc/>`_.
