..
  SPDX-FileCopyrightText: 2019-2023 The PyPSA-Eur Authors

  SPDX-License-Identifier: CC-BY-4.0

##########################################
Building Electricity Networks
##########################################

The preparation process of the PyPSA-Eur energy system model consists of a group of ``snakemake``
rules which are briefly outlined and explained in detail in the sections below.

Not all data dependencies are shipped with the git repository.
Instead we provide separate data bundles which can be obtained
using the ``retrieve*`` rules (:ref:`data`).
Having downloaded the necessary data,

- :mod:`build_shapes` generates GeoJSON files with shapes of the countries, exclusive economic zones and `NUTS3 <https://en.wikipedia.org/wiki/Nomenclature_of_Territorial_Units_for_Statistics>`_ areas.
- :mod:`build_cutout` prepares smaller weather data portions from `ERA5 <https://www.ecmwf.int/en/forecasts/datasets/reanalysis-datasets/era5>`_ for cutout ``europe-2013-era5`` and SARAH for cutout ``europe-2013-sarah``.

With these and the externally extracted ENTSO-E online map topology
(``data/entsoegridkit``), it can build a base PyPSA network with the following rules:

- :mod:`base_network` builds and stores the base network with all buses, HVAC lines and HVDC links, while
- :mod:`build_bus_regions` determines `Voronoi cells <https://en.wikipedia.org/wiki/Voronoi_diagram>`_ for all substations.

Then the process continues by calculating conventional power plant capacities, potentials, and per-unit availability time series for variable renewable energy carriers and hydro power plants with the following rules:

- :mod:`build_powerplants` for today's thermal power plant capacities using `powerplantmatching <https://github.com/FRESNA/powerplantmatching>`_ allocating these to the closest substation for each powerplant,
- :mod:`build_natura_raster` for rasterising NATURA2000 natural protection areas,
- :mod:`build_ship_raster` for building shipping traffic density,
- :mod:`build_renewable_profiles` for the hourly capacity factors and installation potentials constrained by land-use in each substation's Voronoi cell for PV, onshore and offshore wind, and
- :mod:`build_hydro_profile` for the hourly per-unit hydro power availability time series.

The central rule :mod:`add_electricity` then ties all the different data inputs
together into a detailed PyPSA network stored in ``networks/elec.nc``.

.. _busregions:

Rule ``build_bus_regions``
=============================

.. automodule:: build_bus_regions

.. _cutout:

Rule ``build_cutout``
=============================

.. automodule:: build_cutout


Rule ``prepare_links_p_nom``
===============================

.. automodule:: prepare_links_p_nom

.. _natura:

Rule ``build_natura_raster``
===============================

.. automodule:: build_natura_raster


.. _base:

Rule ``base_network``
=============================

.. automodule:: base_network

.. _shapes:

Rule ``build_shapes``
=============================

.. automodule:: build_shapes


.. _powerplants:

Rule ``build_powerplants``
=============================

.. automodule:: build_powerplants


.. _electricity_demand:

Rule ``build_electricity_demand``
==================================


.. automodule:: build_electricity_demand

.. _monthlyprices:

Rule ``build_monthly_prices``
=============================

.. automodule:: build_monthly_prices

.. _ship:

Rule ``build_ship_raster``
===============================


.. automodule:: build_ship_raster

.. _availabilitymatrixmdua:

Rule ``determine_availability_matrix_MD_UA``
============================================

.. automodule:: determine_availability_matrix_MD_UA

.. _renewableprofiles:

Rule ``build_renewable_profiles``
====================================

.. automodule:: build_renewable_profiles


.. _hydroprofiles:

Rule ``build_hydro_profile``
===============================

.. automodule:: build_hydro_profile

.. _electricity:

Rule ``add_electricity``
=============================

.. automodule:: add_electricity
