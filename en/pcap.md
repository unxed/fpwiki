# pcap

The PCAP libraries, libpcap and winpcap provide a portable framework for low-level network monitoring on Linux and Windows operating systems, respectively. This package provides a header-unit for PCAP libraries. 

## Supported Functions and Procedures

  * Column 'pcap.pp': Whether this PCAP function is included in the pcap.pp headers
  * Column 'libpcap': Whether this function is applies to libpcap or not
  * Column 'winpcap': Whether this function is applies to winpcap or not

**pcap.pp** | **libpcap** | **winpcap** | **Name** | **Description**  
---|---|---|---|---  
X | X | X | pcap_breakloop | Set a flag that will force pcap_dispatch() or pcap_loop() to return rather than looping.   
X | X | X | pcap_close | Close the files associated with p and deallocates resources.   
X | X | X | pcap_compile | Compile a packet filter, converting an high level filtering expression (see Filtering expression syntax) in a program that can be interpreted by the kernel-level filtering engine.   
X | X | X | pcap_compile_nopcap | Compile a packet filter without the need of opening an adapter. This function converts an high level filtering expression (see Filtering expression syntax) in a program that can be interpreted by the kernel-level filtering engine.   
|  | X | pcap_createsrcstr | Accept a set of strings (host name, port, ...), and it returns the complete source string according to the new format (e.g. 'rpcap://1.2.3.4/eth0').   
X | X | X | pcap_datalink | Return the link layer of an adapter.   
X | X | X | pcap_datalink_name_to_val | Translates a data link type name, which is a DLT_ name with the DLT_ removed, to the corresponding data link type value. The translation is case-insensitive. -1 is returned on failure.   
X | X | X | pcap_datalink_val_to_description | Translates a data link type value to a short description of that data link type. NULL is returned on failure.   
X | X | X | pcap_datalink_val_to_name | Translates a data link type value to the corresponding data link type name. NULL is returned on failure.   
X | X | X | pcap_dispatch | Collect a group of packets.   
X | X | X | pcap_dump | Save a packet to disk.   
X | X | X | pcap_dump_close | Closes a savefile.   
Commented Out | X | X | pcap_dump_file | Return the standard I/O stream of the 'savefile' opened by pcap_dump_open().   
X | X | X | pcap_dump_flush | Flushes the output buffer to the ``savefile,_so that any packets written with pcap_dump() but not yet written to the ``savefile_ will be written. -1 is returned on error, 0 on success.   
X | X |  | pcap_dump_fopen | Open a file to which to write packets   
X | X | X | pcap_dump_ftell | Return the file position for a "savefile".   
X | X | X | pcap_dump_open | Open a file to write packets.   
Commented Out | X | X | pcap_file | Return the standard stream of an offline capture. Deprecated.   
X |  |  | pcap_fileno | NOTE: Does not exist in current libpcap or winpcap documentation.   
X | X | X | pcap_findalldevs | Construct a list of network devices that can be opened with pcap_open_live().   
|  | X | pcap_findalldevs_ex | Create a list of network devices that can be opened with pcap_open().   
X | X | X | pcap_freealldevs | Free an interface list returned by pcap_findalldevs().   
X | X | X | pcap_freecode | Free a filter.   
|  | X | pcap_get_airpcap_handle | Returns the AirPcap handler associated with an adapter. This handler can be used to change the wireless-related settings of the CACE Technologies AirPcap wireless capture adapters.   
X | X | X | pcap_geterr | Return the error text pertaining to the last pcap library error.   
|  | X | pcap_getevent | Return the handle of the event associated with the interface p.   
X | X | X | pcap_getnonblock | Get the "non-blocking" state of an interface.   
X | X |  | pcap_inject | Transmit a packet   
X | X | X | pcap_is_swapped | Returns true if the current savefile uses a different byte order than the current system.   
X | X | X | pcap_lib_version | Returns a pointer to a string giving information about the version of the libpcap library being used; note that it contains more information than just a version number.   
X | X | X | pcap_list_datalinks | List datalinks   
|  | X | pcap_live_dump | Save a capture to file.   
|  | X | pcap_live_dump_ended | Return the status of the kernel dump process, i.e. tells if one of the limits defined with pcap_live_dump() has been reached.   
X | X | X | pcap_lookupdev | Return the first valid device in the system. Deprecated.   
X | X | X | pcap_lookupnet | Return the subnet and netmask of an interface. Deprecated.   
X | X | X | pcap_loop | Collect a group of packets.   
X | X | X | pcap_major_version | Return the major version number of the pcap library used to write the savefile.   
X | X | X | pcap_minor_version | Return the minor version number of the pcap library used to write the savefile.   
X | X | X | pcap_next | Return the next available packet.   
X | X | X | pcap_next_ex | Read a packet from an interface or from an offline capture.   
|  | X | pcap_offline_filter | Returns if a given filter applies to an offline packet.   
|  | X | pcap_open | Open a generic source in order to capture / send (WinPcap only) traffic.   
X | X | X | pcap_open_dead | Create a pcap_t structure without starting a capture.   
X | X | X | pcap_open_live | Open a live capture from the network.   
X | X | X | pcap_open_offline | Open a savefile in the tcpdump/libpcap format to read packets.   
|  | X | pcap_parsesrcstr | Parse the source string and returns the pieces in which the source can be split.   
X | X | X | pcap_perror | Print the text of the last pcap library error on stderr, prefixed by prefix.   
|  | X | pcap_remoteact_accept | Block until a network connection is accepted (active mode only).   
|  | X | pcap_remoteact_cleanup | Clean the socket that is currently used in waiting active connections.   
|  | X | pcap_remoteact_close | Drop an active connection (active mode only).   
|  | X | pcap_remoteact_list | Return the hostname of the host that have an active connection with us (active mode only).   
X | X | X | pcap_sendpacket | Send a raw packet.   
|  | X | pcap_sendqueue_alloc | Allocate a send queue.   
|  | X | pcap_sendqueue_destroy | Destroy a send queue.   
|  | X | pcap_sendqueue_queue | Add a packet to a send queue.   
|  | X | pcap_sendqueue_transmit | Send a queue of raw packets to the network.   
X | X | X | pcap_set_datalink | Set the current data link type of the pcap descriptor to the type specified by dlt. -1 is returned on failure.   
X |  | X | pcap_setbuff | Set the size of the kernel buffer associated with an adapter.   
X | X |  | pcap_setdirection | Set the direction for which packets will be captured. Appears to be no longer supported in either library.   
X | X | X | pcap_setfilter | Associate a filter to a capture.   
X |  | X | pcap_setmintocopy | Set the minumum amount of data received by the kernel in a single call.   
X |  | X | pcap_setmode | Set the working mode of the interface p to mode.   
X | X | X | pcap_setnonblock | Switch between blocking and nonblocking mode.   
|  | X | pcap_setsampling | Define a sampling method for packet capture.   
X | X | X | pcap_snapshot | Return the dimension of the packet portion (in bytes) that is delivered to the application.   
X | X | X | pcap_stats | Return statistics on current capture.   
|  | X | pcap_stats_ex | Return statistics on current capture.   
X | X | X | pcap_strerror | Provided in case strerror() isn't available.   
  
## Example Programs

(This is a WIP - jdlinke 4 June 2015)

---

_Source: [https://wiki.freepascal.org/pcap](https://web.archive.org/web/20210116050109/https://wiki.freepascal.org/pcap)_
