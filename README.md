# RECHNERNETZE LABOR 3

## Aufgabe 1.1

```
Wichtige Funktionen vom Server

TCP socket : 			      95: socket()
Registrierung : 		    113: bind()
Schließt den Socket :   116: close(ss)
Buffer : 			          127: listen()
Verbindungsaufbau : 	  150: accept()
Lese Buffer : 			    219: read()
Schreibe an Client :	 	270: write()

Wichtige Funktionen des Clients

TCP socket : 			      112: socket()
Registrierung : 		    149: bind()
Schließt Socket : 		  155: close(cs)
Schreibe an Server :		244: write()
Lese Buffer : 			    262: read()
```

## Aufgabe 1.2

Filter: tcp.port == 9001 or icmp

### Wie wurde die MSS berechnet?
1500 Bytes - 20 Bytes [IP] - 20 Bytes [TCP] = 1460
Da wir aber noch Tunneln, 1460 - 24 Bytes [Tunneling] = 1436 Bytes

### Was sagt die ICMP Nachricht vom Router?
Der Router sendet eine ICMP Nachricht, dass eine Fragmentierung notwendig ist.

```
183	19.314823	134.108.11.254	134.108.8.36	ICMP	70	Destination unreachable (Fragmentation needed)	0.000623
```

### Warum soll fragmentiert werden?
Da Ethernet max. 1500 Bytes pro Paket versenden kann.

### Wie sieht TCP Segmentierung aus? Ist diese Segmentierung sinnvoll?
Es wird in zwei Datenpakete fragmentiert, einmal mit 1424 Bytes
und einmal mit 24 Bytes, wodurch unnötig Datenverkehr produziert wird.


Mit Fragmentierung:

```
183	19.314823	134.108.11.254	134.108.8.36	ICMP	70	Destination unreachable (Fragmentation needed)	0.000623

184	19.314851	134.108.8.36	134.108.190.10	TCP	1490	[TCP Retransmission] 46962 → 9001 [ACK] Seq=1 Ack=1 Win=2920 Len=1424 TSval=11507963 TSecr=2383618662	0.000028

185	19.314861	134.108.8.36	134.108.190.10	TCP	90	[TCP Retransmission] 46962 → 9001 [ACK] Seq=1425 Ack=1 Win=2920 Len=24 TSval=11507963 TSecr=2383618662	0.000010

186	19.314867	134.108.8.36	134.108.190.10	TCP	1490	[TCP Retransmission] 46962 → 9001 [ACK] Seq=1449 Ack=1 Win=2920 Len=1424 TSval=11507963 TSecr=2383618662	0.000006

187	19.314872	134.108.8.36	134.108.190.10	TCP	90	[TCP Window Full] [TCP Retransmission] 46962 → 9001 [PSH, ACK] Seq=2873 Ack=1 Win=2920 Len=24 TSval=11507963 TSecr=2383618662	0.000005

188	19.315528	134.108.190.10	134.108.8.36	TCP	66	9001 → 46962 [ACK] Seq=1 Ack=1425 Win=2896 Len=0 TSval=2383627172 TSecr=11507963	0.000656

189	19.315558	134.108.8.36	134.108.190.10	TCP	1490	[TCP Window Full] 46962 → 9001 [ACK] Seq=2897 Ack=1 Win=2920 Len=1424 TSval=11507964 TSecr=2383627172	0.000030

190	19.315566	134.108.190.10	134.108.8.36	TCP	66	9001 → 46962 [ACK] Seq=1 Ack=1449 Win=2896 Len=0 TSval=2383627172 TSecr=11507963	0.000008

191	19.315577	134.108.8.36	134.108.190.10	TCP	90	[TCP Window Full] 46962 → 9001 [ACK] Seq=4321 Ack=1 Win=2920 Len=24 TSval=11507964 TSecr=2383627172	0.000011

192	19.315583	134.108.190.10	134.108.8.36	TCP	66	9001 → 46962 [ACK] Seq=1 Ack=2897 Win=2896 Len=0 TSval=2383627172 TSecr=11507963	0.000006

193	19.315594	134.108.8.36	134.108.190.10	TCP	1490	46962 → 9001 [ACK] Seq=4345 Ack=1 Win=2920 Len=1424 TSval=11507964 TSecr=2383627172	0.000011

194	19.315598	134.108.8.36	134.108.190.10	TCP	90	[TCP Window Full] 46962 → 9001 [PSH, ACK] Seq=5769 Ack=1 Win=2920 Len=24 TSval=11507964 TSecr=2383627172	0.000004

197	19.316218	134.108.190.10	134.108.8.36	TCP	66	9001 → 46962 [ACK] Seq=1 Ack=4345 Win=2896 Len=0 TSval=2383627173 TSecr=11507964	0.000203

198	19.316245	134.108.8.36	134.108.190.10	TCP	1490	46962 → 9001 [ACK] Seq=5793 Ack=1 Win=2920 Len=1424 TSval=11507964 TSecr=2383627173	0.000027
```

Ohne Fragmentierung:

```
232	19.318494	134.108.8.36	134.108.190.10	TCP	1490	46962 → 9001 [PSH, ACK] Seq=15905 Ack=1 Win=2920 Len=1424 TSval=11507966 TSecr=2383627175	0.000012

237	19.319163	134.108.190.10	134.108.8.36	TCP	66	9001 → 46962 [ACK] Seq=1 Ack=17329 Win=2896 Len=0 TSval=2383627176 TSecr=11507966	0.000103

238	19.319180	134.108.8.36	134.108.190.10	TCP	1490	46962 → 9001 [ACK] Seq=17329 Ack=1 Win=2920 Len=1424 TSval=11507967 TSecr=2383627176	0.000017

239	19.319187	134.108.8.36	134.108.190.10	TCP	1490	46962 → 9001 [PSH, ACK] Seq=18753 Ack=1 Win=2920 Len=1424 TSval=11507967 TSecr=2383627176	0.000007

244	19.320019	134.108.190.10	134.108.8.36	TCP	66	9001 → 46962 [ACK] Seq=1 Ack=20177 Win=2896 Len=0 TSval=2383627177 TSecr=11507967	0.000009

245	19.320034	134.108.8.36	134.108.190.10	TCP	1490	46962 → 9001 [ACK] Seq=20177 Ack=1 Win=2920 Len=1424 TSval=11507968 TSecr=2383627177	0.000015

246	19.320042	134.108.8.36	134.108.190.10	TCP	1490	46962 → 9001 [PSH, ACK] Seq=21601 Ack=1 Win=2920 Len=1424 TSval=11507968 TSecr=2383627177	0.000008

251	19.320869	134.108.190.10	134.108.8.36	TCP	66	9001 → 46962 [ACK] Seq=1 Ack=23025 Win=2896 Len=0 TSval=2383627178 TSecr=11507968	0.000009

252	19.320884	134.108.8.36	134.108.190.10	TCP	1490	46962 → 9001 [ACK] Seq=23025 Ack=1 Win=2920 Len=1424 TSval=11507969 TSecr=2383627178	0.000015
```



