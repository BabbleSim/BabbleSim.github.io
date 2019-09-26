## Converting BLE activity to pcap files

The simulator 2.4GHz Phy radio dump can be converted into a pcap format readable by Wireshark and other tools.

To merge the TX activity from all devices into a single pcap trace do:

```sh
components/ext_2G4_phy_v1/dump_post_process/csv2pcap -o mytrace.pcap results/<sim_id>/d_2G4*.Tx.csv
```
Replace paths as necessary.

Alternatively one can merge RX and TX activity from a single device to get a trace as seen by that device:
```sh
components/ext_2G4_phy_v1/dump_post_process/csv2pcap -o mytrace.pcap results/<sim_id>/d_2G4_00.{Rx,Tx}.csv
```

However be careful not to merge Rx and Tx files from different devices or you'll get duplicated records.

Note that the Phy cannot be run with the `-nodump` option (otherwise it will not dump the radio activity to the results folder)
