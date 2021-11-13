About
=====================

On these pages, you find tutorials for Matlab software to work
with geophysical electromagnetic time series of various kinds (see Objectives below). The code is
written in object-oriented Matlab (as to our best knowledge) and can be invoked from scripts
that control the processing flow. How to write scripts for different objectives is described
in the respective tutorials.

Neither the software nor the help pages are complete
but we try to update them (dis)continously. The code is being developed at the Institute of Geophysics, University of Münster, and primarily
tailored to our own needs in teaching and research. However, some tools might be of use for other
parties as well.

It is not our goal to initiate an open-source project on processing of electromagnetic data (because
we don't have the capacity to administrate such a project), but everyone is free to use what is available.
Not warranty is provided though.

Our primary goals has been to provide a framework that allows to read and **work with different data types** with
a **minimal effort in own coding** (such as writing routines to read particular data formats)
and to **avoid the pitfalls** that newcomers to the field typically fall into (such as working with time stamps).
Instead, it is the hoped that the tools allow to concentrate the user on actual data problems and processing algorithms.

Moreover, we have the aim to provide typical workflows for processing electromagnetic data, and to have
them documented with sufficient detail to allow students to get started in a reasonable amount of time.
For instance, **for students** carrying out magnetotelluric measurements during a field course, these
help pages hopefully provide sufficient support to **guide them through the standard processing steps**.


Objectives
-----------

The primary objective of processing is the estimation of response functions (or transfer functions) from
time series measurements of (geophysical) fields. Response functions constitute the
observational data used in modelling and inversion; estimating best-quality response functions is a crucial step
in data analysis and pre-requiste for meaningful applications.

The objective of the software package is to read and process time series recordings obtained in EM surveys and to
deliver **statistical estimates of response functions** in a wider sense. The following surveying methods are covered:

* Magnetotellurics (MT)
* Controlled source electromagnetics (CSEM)
* Semi-airborne electromagnetics (s-AEM)
* Audio-frequency magnetics (AFMAG)
* Direct Current geoelectrics (DC)

In addition, we have implemented some tools to process data acquired in

* Airborne geomagnetics

which relies on the same framework of Matlab classes.

Tentatively, we aim at including global induction studies, particular Sq-variations, and time domain electromagnetics.

Response functions
-------------------

Response functions are frequency domain transfer functions that describe a linear system. The equivalent time domain
response function is denoted as the Green's function of the system. For the afore listed applications, we can define
the response function :math:`\cal T` from the multi-linear system

.. math::

   {\cal O}_{i}(\omega)=\sum_{j}{\cal T}_{i,j}(\omega){\cal I}_{j}(\omega)+{\cal E}_{i}(\omega)\,\,\,.

Here, :math:`{\cal O}_{i}` is the :math:`i\textrm{-th}` output channel of the system, and :math:`{\cal I}_{j}` are
the :math:`j=1,...,J` input channels. :math:`{\cal E}_{i}` is an observation error in the output channel.

The most simple response function is Ohm's resistance :math:`R` estimated in **direct current (DC) geoelectrical applications**. From Ohm's law, we have

.. math::

   u=Ri\,\,\,,

where :math:`R` is usually obtained from the ratio of stacked voltages :math:`u` and currents :math:`i`.
However, it should be noted that in DC applications, the polarity of the injected current :math:`i` is steadily
switched. It means that the current :math:`i=i(t)` is effectively time-dependent (although at low rate) and as
such the observed voltage :math:`u=u(t)` is also time-dependent. Hence, a frequency-dependent response function
:math:`R(\omega)` can be estimated which satisfies

.. math::

   U(\omega)=R(\omega)I(\omega)+\varepsilon(\omega)

As the response approaches DC conditions, the frequency dependency flattens and the imaginary part vanishes.
In the static limit, :math:`\lim_{\omega\rightarrow0}R(\omega)=R`.

The above linear relation is similar to response function in **controlled source EM (CSEM) applications**.
However, in CSEM, we consider both electric and magnetic fields as output channels of the system, and we are
interested in the frequency characteristics of the response function. Hence, in CSEM, we have the uni-linear
relationships

.. math::

   E_{x,y}(\omega)	={\cal E}_{x,y}(\omega)I(\omega)+\varepsilon_{x,y}(\omega)

   B_{x,y,z}(\omega)	={\cal B}_{x,y,z}(\omega)I(\omega)+\varepsilon_{x,y}(\omega)

with the electric and magnetic transfer functions :math:`{\cal E}_{x,y}(\omega)` and :math:`{\cal B}_{x,y}(\omega)`,
respectively.

In a similar fashion, transfer functions are estimated from **semi-airborne EM surveys**. The main difference
is that the receivers are moving, posing additional challenges mainly in preparing the output channel data.
Semi-airborne EM processing is probably the most demanding processing task, and it is likely that our tools are
not general enough to account for other hardwares than the ones we have in use.

In **magnetotellurics (MT)**, the output channel is typically one electric field component, :math:`E_{x}` or :math:`E_{y}`,
or the vertical magnetic flux, :math:`B_{z}`, and the input channels are the horizontal magnetic flux
components, :math:`B_{x}` and :math:`B_{y}`. The commonly used notation is

.. math::

   E_{x}	=Z_{xx}B_{x}/\mu_{0}+Z_{xy}B_{y}/\mu_{0}+\varepsilon_{x}\,\,\,,

   E_{y}	=Z_{yx}B_{x}/\mu_{0}+Z_{yy}B_{y}/\mu_{0}+\varepsilon_{y}\,\,\,,

   B_{z}	=T_{zx}B_{x}+T_{zy}B_{y}+\varepsilon_{z}\,\,\,,

where :math:`Z_{ij}` are the elements of the impedance tensor and :math:`T_{zj}` are the vertical magnetic transfer
function. Note that instead of relating :math:`E_{x}`, :math:`E_{y}`, or :math:`B_{z}` to the horizontal magnetic
flux components at the same location (or site in the sense of stationary recordings, see below), any other
bi-linear system of equation can be set up from channels recorded at different locations. We only require
to have two (orthogonal) components at the same point as the input channels.

Inter-station transfer functions thus include impedances (”E over H”), admittances (”H over E”), magnetic
transfer functions (”H over H”) and telluric transfer (“E over E”) functions.

In a more general errors-in-variables approach, we collect all observational channels in a data matrix
:math:`{\cal C}` and approximate it in a low-dimensional subspace :math:`{\cal A}(\omega)` with particular
realizations :math:`\alpha(\omega)`, i.e.,

.. math::

   {\cal C}(\omega)={\cal A}(\omega)\alpha(\omega)+{\cal E}(\omega)\,\,\,.

Response functions are then to be extracted from the low-dimensional basis :math:`{\cal A}(\omega)`.


Contact
--------------------------

| **Prof. Dr. Michael Becken**
| Westfälische Wilhelms-Universität
| Institut für Geophysik
| Corrensstraße 24
| 48149 Münster
| michael.becken@uni-muenster.de
