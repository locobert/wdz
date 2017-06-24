# wdz
Pcaps et al. for my Wireshark development

# Pcap
* `6LoWPAN.pcap`: 'IPv6 over IEEE 802.15.4.' from <https://wiki.wireshark.org/SampleCaptures>
* `6tisch_6top_example.pcapng`: 'The IETF 6TiSCH Working Group implements the IEEE 802.15.4 Std, using the MAC-TSCH operation mode. Here, plenty of information go into some fields called Information Elements. This pcap file shows you all the information the nodes exchange in order to set and to maintain the network.' from <https://bugs.wireshark.org/bugzilla/show_bug.cgi?id=13005>
* `IEEE802.15.4-2015-Encrypted.pcapng`: example frame for 802.15.4-2015 encryption (cf. also <https://bugs.wireshark.org/bugzilla/show_bug.cgi?id=13805>)
	- Decryption key: 0102030405060708090a0b0c0d0e0f10
	- Key Index: 3
* `ie-test.pcap`: generated PCAP to test IE (error) handling

# Dump
For each pcap, the `.txt` file contains the output of `tshark` and the `.V.txt` file contains the output of `tshark -Vx`.

# Src
The source of the 802.15.4 dissector used for generating the dump.

# IE Test
The pcap contains the following frames with the first header IE ('msg') using the reserved ID 0xff and containing a short description of the use case.

- msg('OK HT2') + HIE(:HT2) + SOME_DATA
- msg('HT2 too long') + HIE(:HT2, '77') + SOME_DATA
- msg('OK TC +5 +ACK') + HIE(:TIME_CORRECTION, '0500')
- msg('OK TC +5 NACK') + HIE(:TIME_CORRECTION, '0580')
- msg('OK TC -7 +ACK') + HIE(:TIME_CORRECTION, 'f90f')
- msg('OK TC -7 NACK') + HIE(:TIME_CORRECTION, 'f98f')
- msg('Illegal TC value') + HIE(:TIME_CORRECTION, '0390')
- msg('TC too  short') + HIE(:TIME_CORRECTION, '00')
- msg('TC too  short 2') + HIE(:TIME_CORRECTION, '00') + HIE(:HT2) + SOME_DATA
- msg('TC too  long') + HIE(:TIME_CORRECTION, '050077')
- msg('OK CSL  no RT') + HIE(:CSL, '00010002')
- msg('OK CSL  with RT') + HIE(:CSL, '000100020003')
- msg('CSL too long') + HIE(:CSL, '00010002000377')
- msg('CSL too short') + HIE(:CSL, '000100')
- msg('CSL ill. length') + HIE(:CSL, '0001000200')
- msg('OK HT1  PT Data') + HIE(:HT1) + PIE(:PT) + SOME_DATA
- msg('HT1 too long') + HIE(:HT1, '77') + PIE(:PT) + SOME_DATA
- msg('HT1 no  PIE') + HIE(:HT1)
- msg('HT1 PIE short') + HIE(:HT1) + "\x77"
- msg('PT too  long') + HIE(:HT1) + PIE(:PT, '77')
- msg('OK      multi') + HIE(:TIME_CORRECTION, '0500') + HIE(:CSL, '00010002') + HIE(:TIME_CORRECTION, 'f98f') + HIE(:CSL, '000100020003') + HIE(:HT1) + PIE(:PT) + SOME_DATA
