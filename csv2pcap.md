## Converting BLE activity to pcap & pcapng files

The simulator 2.4GHz Phy radio dump can be converted into the pcap and pcapng format which is readable by Wireshark and other tools.

To merge the TX activity from all devices into a single pcapng trace do:

```sh
components/ext_2G4_phy_v1/dump_post_process/csv2pcapng -o mytrace.pcap results/<sim_id>/d_2G4*.Tx.csv
```
Replace paths as necessary.

Check the script options (with `-h`) for more info.

Alternatively one can merge RX and TX activity from a single device to get a trace as seen by that device:
```sh
components/ext_2G4_phy_v1/dump_post_process/csv2pcapng -o mytrace.pcap results/<sim_id>/d_2G4_00.{Rx,Tx}.csv
```

However be careful not to merge Rx and Tx files from different devices or you'll get duplicated records.

Note that the Phy cannot be run with the `-nodump` option (otherwise it will not dump the radio activity to the results folder)

It is also possible to generate pcap files, but as pcap files only support 1 type of physical layer at a time (BLE or 15.4)
you will need to chose which you want to generate, and call the respective script (`csv2pcap` or `csv2pcap_15.4.py`).
