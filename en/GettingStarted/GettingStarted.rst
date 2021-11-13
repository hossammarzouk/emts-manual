.. _gettingstarted:

Getting Started
============================

.. Start of roles definitions

.. raw:: html

    <style>
    .red {color:red}
    .blue {color:blue}
    .special{color:#00FF00; background-color:black;font-weight:bold}
    </style>

.. role:: red
.. role:: blue
.. role:: special

.. End of roles definitions

Download and installation
--------------------------

to come

Preparation of data
--------------------------

Folder structure
++++++++++++++++++++

Data are to be organized in a particular folder structure of the type::

  ./<project>/<site>/ts/<tag>/<recorder>/<run>/<time series data files>

For example, within the hypothetic project ``MT.Project.2020``, measurements were carried out at several
stations. A site ``ESL``, we have recorded continuously, and at ``ARN``, the recording was interrupted,
resulting in two runs. For both sites, an ``ADU`` system was used (see :ref:`recordingsystems` for a summary on supported devices). The ``ADU`` stores the data for
continuous measurements in one or multiple ``*.ats`` files within a folder named ``meas_<date>``, where ``<date>`` is close to
the start time of the recording. A separate run is stored in a separate ``meas_<date>`` folder.
To organize the data in our default folder structure, combine them into one project as follows: ::

  ./MT.Project.2020/ESL/ts/adc/ADU/meas_<date>/*.ats
  ./MT.Project.2020/ARN/ts/adc/ADU/meas_<date1>/*.ats
  ./MT.Project.2020/ARN/ts/adc/ADU/meas_<date2>/*.ats

In addition, assume that we operated an ``EDE`` data logger at a site near ``ARN``. This site has the name ``ARN-E01``.
To add these data to the same project (``EDE`` produce measurement folders with the same ``meas_<date>`` convention,
but with file extension ``*.ets``), copy them into: ::

  ./MT.Project.2020/ARN-E01/ts/adc/EDE/meas_<date>/*.ets

Extra project folders and files might be required for processing. For MT measurements,
we require calibration files for the magnetometers, for example. Put them (as the default) into: ::

  ./<project>/s/<calibration files>

Information on calibration files is provided in the section *Calibration files*.

Further auxiliary default folder names are: ::

  ./<project>/transmitter/<transmitter geometry definition files>
  ./<project>/dem/<digital elevation data>
  ./<project>/def/<processing definition files>

More details on the different data loggers and how to organize the data files are provided below and in the section :ref:`recordingsystems`.

Project, site, tag, run, and more: What is what?
+++++++++++++++++++++++++++++++++++++++++++++++++

**Project**

A project contains all data related to a project. The project is simply a folder denoted as::

  ./<project>

In general, data are organized in sites and runs, where different processing steps are
collected in separate tags. In addition, a project requires up-to-date calibration functions,
and - depending on the type of survey - transmitter geometry information and digital elevation model.
The detailed meaning of these items and the sub-structure to organize data within a project is outlined below.

**Site**

The term site is used as an identifier for a unique recording objective. We distinguish the following types of sites:

.. container:: custom

  * stationary sites include MT receivers, telluric receivers, current recordings for a controlled source survey: The common
    characteristic for a stationary site is that it is located at a unique location (MT, telluric) or has a unique geometry (transmitter)
  * moving sites include airborne receiver and IMU recordings: The common characteristic for a moving site
    is that it is associated with a single transmitter geometry (semi-airborne EM) or related
    to a common survey domain (AFMAG, Geomagnetic surveying). For AFMAG and Geomagnetic surveying,
    separate flights can be treated either as separate sites or separate runs.

At a particular site, we typically use the same hardware throughout. However, under certain circumstances,
multiple hardwares are possible, and even multiple recorders are permitted at the same site. However,
it should be taken care of that multiple recording systems do not generate overlapping data,
e.g. a particular electric channel can only be recorded once at a time at a single site and
sampling rate. Hence, if recorders are doing the same (e.g. same channel, same sampling rate,
same or overlapping time) data should be better stored in separate sites.

**Tag**

A time series tag provides a way to distinguish overlapping recordings at the same site. It
is introduced to be used for different processing steps. For example, let us copy the original
recordings in a subdirectory ``adc``, and let us store some filtered data in a subdirectory ``proc``.
``adc`` and ``proc`` denote tags and must be set to specify data access. Tags are also transferred to frequency domain,
i.e., a folder tree storing spectral data will maintain the tag of the corresponding time series folder tree.

**Recorder**

A recorder name indicates the principal hardware used for recording data, and it should
be used as name of the subfolder archiving separate runs recorded with this particular
recording system. Recorders are ``ADU`` ``EDE`` ``..`` We encode the Recorder name, or some abbreviation,
in some way in output filenames (see :ref:`recordingsystems` for details).

**Run**

A run is a continuous recording of a number of channels at a fixed sampling rate. Data of
separate runs are stored in separate subdirectories of the respective recorder folder.
Typical folder names holding a run are of type ``meas*``.

**Calibration files**

to come

**Transmitter geometry**

to come

**Digital elevation models**

to come

Initializing a project
--------------------------

Common to all data in one project is that they reside in the same project folder and that they refer to a common reference time.
Although we use UTC-time as far as possible, a time for internal time referencing must also be specified. Normally, the
reference time should be shortly before the beginning of the survey, but this is not a must. The
main use of the reference time is to align time windows that are transformed into spectral domain.
This is required to cover exactly the same time span for single FFT estimates from multiple recorders with distinct recording times.

The reference time is specified in ``datetime`` format, e.g. as ``datetime([yyyy, MM, dd])``. Both the project folder and the
reference time are passed as arguments to initialize a survey. For instance, to initialize an MT survey, use:

.. code:: matlab

  mt  = MT('/Volumes/INTENSO_4TB/MT.Project.2020', datetime(2020,10,24));

Here, ``mt`` is an object derived from the class definition ``MT``. The initialization routine throws a
message into the matlab command window that should look like this:

.. code:: text

  + MT Project directory is /Volumes/INTENSO_4TB/MT.Sauerland.2021
  reference time is 2021-10-24 00-00-00

The same type of initialization applies to other processing classes. See the respective tutorials.

Design of a processing script
-------------------------------

Data access and processing is generally controlled through scripts. A script has typically the following sections:

.. code:: matlab

  clc;
  %% Initialize project
  % ..
  ..
  %% Import time series
  % ..
  ..
  %% Compute spectra
  % ..
  ..
  %% Compute transfer functions
  % ..
  ..

where ``% ..`` is a placeholder for a comment, and ``..`` a placeholder for code. We recommend
to save this script in your project folder, or a subfolder for scripts, and possibly using a name
that relates to the particular processing task. We choose ``./MT.Sauerland.2021/scripts/proc_ESL.m``.

The remainder of these tutorials is how to fill this script with content.

Some nomenclature
--------------------------

* Local site, tag, sampling rate:

  to come

* Base site, tag, sampling rate:

  to come

* Reference site, tag, sampling rate:

  to come

* Channels:

  to come

* Input Channels:

  to come

* Output Channels

  to come

* Time

  to come

* Decimation

  to come

* Band setup


Software from external parties
------------------------------

Depending on the particular application, we make use of tools from third parties, or expect
that the data have been prepared with  proprietary software from instrument manufacturers. The
list below provides an overview of third-party software.

* ``m_map``. Mapping toolbox, primarily used here for projecting geographic coordinates to UTM and vice versa.

* ``igrf.m``. International geomagnetic reference field, IGRF. Drew Compston (2021). International
  Geomagnetic Reference Field (IGRF): MATLAB Central File Exchange. Retrieved May 16, 2021:
  https://www.mathworks.com/matlabcentral/fileexchange/34388-international-geomagnetic-reference-field-igrf-model .
  The latest coefficients can be found on:
  https://ccmc.gsfc.nasa.gov/pub/modelweb/geomagnetic/igrf/data_files_for_Matlab/ .
  Required by various airborne processing methods.

* ``ardupilog.m``. Handles navigation data from UAS flight controller
  and relies on the external class definition ``ardupilog``, which can be found at:
  https://github.com/Georacer/ardupilog. Required by the ``ArduPilot`` class.

* Geometrics Survey manager

* iMAR software iXCOM-CMD.
