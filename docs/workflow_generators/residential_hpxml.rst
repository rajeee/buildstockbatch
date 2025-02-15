Residential HPXML Workflow Generator
------------------------------------

Configuration Example
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: yaml

    workflow_generator:
      type: residential_hpxml
      args:
        build_existing_model:
          simulation_control_run_period_calendar_year: 2010
          add_component_loads: false

        emissions:
          - scenario_name: Scenario1
            type: CO2
            elec_folder: data/cambium/LRMER_MidCase_15

        utility_bills:
          - scenario_name: Bills1
            pv_compensation_type: NetMetering

        simulation_output_report:
          timeseries_frequency: hourly
          include_timeseries_total_consumptions: true
          include_timeseries_fuel_consumptions: true
          include_timeseries_end_use_consumptions: true
          include_timeseries_emissions: true
          output_variables:
            - name: Zone People Occupant Count

        server_directory_cleanup:
          retain_in_osm: true
          retain_in_idf: false
          retain_eplusout_rdd: true

        debug: false

Arguments
~~~~~~~~~

- ``build_existing_model``: Update the simulation control arguments to the `BuildExistingModel`_ measure. See
  :ref:`hpxml-build-existing-model-defaults` for current defaults.

- ``emissions`` (optional): Add these arguments to the `BuildExistingModel`_ measure for performing emissions calculations.

    - ``scenario_name``: Name of the emissions scenario.
    - ``type``: Type of emission (e.g., CO2, NOx, etc.).
    - ``elec_folder``: Folder of schedule files with hourly electricity emissions factors values. Units are kg/MWh. Path is relative to buildstock_directory's resources folder. File names must contain GEA region names.
    - ``gas_value``: Annual emissions factor for natural gas. Units are lb/MBtu (million Btu).
    - ``propane_value``: Annual emissions factor for propane. Units are lb/MBtu (million Btu).
    - ``oil_value``: Annual emissions factor for fuel oil. Units are lb/MBtu (million Btu).
    - ``wood_value``: Annual emissions factor for wood. Units are lb/MBtu (million Btu).

- ``utility_bills`` (optional): Add these arguments to the `BuildExistingModel`_ measure for performing utility bill calculations.

    - ``scenario_name``: Name of the utility bills scenario.
    - ``elec_fixed_charge``: Monthly fixed charge for electricity.
    - ``elec_marginal_rate``: Marginal rate for electricity. Units are $/kWh.
    - ``gas_fixed_charge``: Monthly fixed charge for natural gas.
    - ``gas_marginal_rate``: Marginal rate for natural gas. Units are $/therm.
    - ``propane_fixed_charge``: Monthly fixed charge for propane.
    - ``propane_marginal_rate``: Marginal rate for propane. Units are $/gallon.
    - ``oil_fixed_charge``: Monthly fixed charge for fuel oil.
    - ``oil_marginal_rate``: Marginal rate for fuel oil. Units are $/gallon.
    - ``wood_fixed_charge``: Monthly fixed charge for wood.
    - ``wood_marginal_rate``: Marginal rate for wood. Units are $/kBtu.
    - ``pv_compensation_type``: Photovoltaic compensation types. Can be NetMetering or FeedInTariff.
    - ``pv_net_metering_annual_excess_sellback_rate_type``: Photovoltaic net metering annual excess sellback rate type. Can be User-Specified or Retail Electricity Cost. Applies if compensation type is NetMetering.
    - ``pv_net_metering_annual_excess_sellback_rate``: Photovoltaic net metering annual excess sellback rate. Applies if compensation type is NetMetering.
    - ``pv_feed_in_tariff_rate``: Photovoltaic annual full/gross feed-in tariff rate. Applies if compensation type is FeedInTariff.
    - ``pv_monthly_grid_connection_fee_units``: Photovoltaic monthly grid connection fee units. Can be $ or $/kW.
    - ``pv_monthly_grid_connection_fee``: Photovoltaic monthly grid connection fee.

- ``measures`` (optional): Add these optional measures to the end of your workflow.

    - ``measure_dir_name``: Name of measure directory.
    - ``arguments``: map of key, value arguments to pass to the measure.

- ``simulation_output_report``: Update the arguments to the `ReportSimulationOutput`_ measure. See
  :ref:`hpxml-sim-output-report-defaults` for current defaults.

  - ``output_variables``: Optionally request EnergyPlus output variables. Do not include key values; by default all key values will be requested.

- ``reporting_measures`` (optional): A list of reporting measures to apply
  to the workflow. Any columns reported by these additional measures will be
  appended to the results csv. Note: For upgrade runs, do not add
  ``ApplyUpgrade`` to the list of reporting measures, doing so will cause run
  to fail prematurely. ``ApplyUpgrade`` is applied automatically when the
  ``upgrades`` key is supplied.

  - ``measure_dir_name``: Name of measure directory.
  - ``arguments``: map of key, value arguments to pass to the measure.

- ``server_directory_cleanup`` (optional): Optionally preserve or delete
  various simulation output files. These arguments are passed directly to
  the `ServerDirectoryCleanup`_ measure in resstock. Please refer to the
  measure arguments there to determine what to set them to in your config file.
  Note that the default behavior is to retain some files and remove others.
  See :ref:`hpxml-server-dir-cleanup-defaults` for current defaults.

- ``debug`` (optional): Optionally enable debug mode. Enabling debug
  mode will preserve all simulation input and output files, including but
  not limited to: in.osm, all EnergyPlus output files, and intermediate
  existing and upgraded files (e.g., OSWs and XMLs).

.. _BuildExistingModel: https://github.com/NREL/resstock/blob/develop/measures/BuildExistingModel/measure.xml
.. _ReportSimulationOutput: https://github.com/NREL/resstock/blob/develop/resources/hpxml-measures/ReportSimulationOutput/measure.xml
.. _ServerDirectoryCleanup: https://github.com/NREL/resstock/blob/develop/measures/ServerDirectoryCleanup/measure.xml

.. _hpxml-build-existing-model-defaults:

Build Existing Model Defaults
.............................

.. include:: ../../buildstockbatch/workflow_generator/residential_hpxml.py
   :code: python
   :start-after: sim_ctl_args = {
   :end-before: }

.. _hpxml-sim-output-report-defaults:

Simulation Output Report Defaults
..................................

.. include:: ../../buildstockbatch/workflow_generator/residential_hpxml.py
   :code: python
   :start-after: sim_out_rep_args = {
   :end-before: }

.. _hpxml-server-dir-cleanup-defaults:

Server Directory Cleanup Defaults
.................................

.. include:: ../../buildstockbatch/workflow_generator/residential_hpxml.py
   :code: python
   :start-after: server_dir_cleanup_args = {
   :end-before: }
