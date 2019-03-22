## Importing simulation activity into the Ellisys Bluetooth analyzer SW

The simulator 2.4GHz Phy radio dump can be converted into a trace readable by the Ellisys Bluetooth Analyzer SW.

To convert the activity from all devices in a given simulation into a trace run (replace paths as necessary):

```
components/ext_2G4_phy_v1/dump_post_process/convert_results_to_ellisysv2.sh results/<sim_id>/d_2G4*.Tx.csv > ~/Trace.bttrp
```

This trace can then be imported into the Ellisys SW: File > Import ; Select Bluetooth packets ; Click Next ; Click Browse ; and select/open the file.

Note that this script will ignore any non BLE activity (as the Ellisys SW cannot process it)

Note that the Phy cannot be run with the `-nodump` option (otherwise it will not dump the radio activity to the results folder)
