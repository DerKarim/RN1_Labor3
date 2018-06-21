# RECHNERNETZE LABOR 3

## Aufgabe 1.1

**Wichtige Funktionen vom Server**

|Beschreibung         |Line |Funktion   |
|---------------------|-----|-----------|
|TCP socket           |95   | socket()  |
|Registrierung        |113  |bind()     |
|Schließt den Socket  |116  |close(ss)  |
|Buffer               |127  |listen()   |
|Verbindungsaufbau    |150  |accept()   |
|Lese Buffer          |219  |read()     |
|Schreibe an Client   |270  |write()    |

**Wichtige Funktionen des Clients**

|Beschreibung         |Line |Funktion   |
|---------------------|-----|-----------|
|TCP socket           |112  | socket()  |
|Registrierung        |149  |bind()     |
|Schließt den Socket  |155  |close(ss)  |
|Schreibe an Server   |244  |listen()   |
|Lese Buffer          |262  |read()     |


### 1.1.1 Ein Server und ein Client

**Versuchsziel:**  
Ausführen der vorgegebenen Programme auf zwei verschiedenen Rechnern. Dokumentation des Datentransfers im Netzwerk mithilfe von Wireshark. Herausfinden woran man erkennt, dass das Programm kein neben läufiger Server ist.  
**Versuchsdurchführung:**  
Man kann in der Konsolenausgabe sehr schön sehen, dass der Server sich immer nur mit einem Client verbindet und auch nur mit einem Client unterhält.

```
No.     Time           Source                Destination           Protocol Length Info                                                            Delta TIme
38    0.087676	134.108.8.37	134.108.8.36	TCP	74	54774 → 9001 [SYN] Seq=0 Win=2920 Len=0 MSS=1460 SACK_PERM=1 TSval=4155936 TSecr=0 WS=1
39    0.000051	134.108.8.36	134.108.8.37	TCP	74	9001 → 54774 [SYN, ACK] Seq=0 Ack=1 Win=2896 Len=0 MSS=1460 SACK_PERM=1 TSval=5790016 TSecr=4155936 WS=1
40    0.000181	134.108.8.37	134.108.8.36	TCP	66	54774 → 9001 [ACK] Seq=1 Ack=1 Win=2920 Len=0 TSval=4155936 TSecr=5790016
...
186   0.000006       134.108.8.36          134.108.8.37          TCP      1514   9001 → 54774 [PSH, ACK] Seq=7241 Ack=60001 Win=2896 Len=1448 TSval=5802527 TSecr=4168448
187   0.000151       134.108.8.37          134.108.8.36          TCP      66     54774 → 9001 [ACK] Seq=60001 Ack=8689 Win=2920 Len=0 TSval=4168448 TSecr=5802527
188   0.000028       134.108.8.36          134.108.8.37          TCP      1514   9001 → 54774 [ACK] Seq=8689 Ack=60001 Win=2896 Len=1448 TSval=5802528 TSecr=4168448
189   0.000006       134.108.8.36          134.108.8.37          TCP      1514   9001 → 54774 [PSH, ACK] Seq=10137 Ack=60001 Win=2896 Len=1448 TSval=5802528 TSecr=4168448
190   0.000182       134.108.8.37          134.108.8.36          TCP      66     54774 → 9001 [ACK] Seq=60001 Ack=11585 Win=2920 Len=0 TSval=4168448 TSecr=5802528
191   0.000042       134.108.8.36          134.108.8.37          TCP      1514   9001 → 54774 [ACK] Seq=11585 Ack=60001 Win=2896 Len=1448 TSval=5802528 TSecr=4168448
192   0.000011       134.108.8.36          134.108.8.37          TCP      1034   9001 → 54774 [PSH, ACK] Seq=13033 Ack=60001 Win=2896 Len=968 TSval=5802528 TSecr=4168448
193   0.000168       134.108.8.37          134.108.8.36          TCP      66     54774 → 9001 [ACK] Seq=60001 Ack=14001 Win=2920 Len=0 TSval=4168449 TSecr=5802528
194   0.000018       134.108.8.37          134.108.8.36          TCP      69     54774 → 9001 [PSH, ACK] Seq=60001 Ack=14001 Win=2920 Len=3 TSval=4168449 TSecr=5802528
195   0.000007       134.108.8.37          134.108.8.36          TCP      66     54774 → 9001 [FIN, ACK] Seq=60004 Ack=14001 Win=2920 Len=0 TSval=4168449 TSecr=5802528
196   0.000032       134.108.8.36          134.108.8.37          TCP      66     9001 → 54774 [ACK] Seq=14001 Ack=60004 Win=2896 Len=0 TSval=5802528 TSecr=4168449
197   0.039637       134.108.8.36          134.108.8.37          TCP      66     9001 → 54774 [ACK] Seq=14001 Ack=60005 Win=2896 Len=0 TSval=5802568 TSecr=4168449
208   0.960503       134.108.8.36          134.108.8.37          TCP      66     9001 → 54774 [FIN, ACK] Seq=14001 Ack=60005 Win=2896 Len=0 TSval=5803528 TSecr=4168449
209   0.000165       134.108.8.37          134.108.8.36          TCP      66     54774 → 9001 [ACK] Seq=60005 Ack=14002 Win=2920 Len=0 TSval=4169449 TSecr=5803528
```

## Aufabe 1.1.2

**Versuchsziel**  
Es wird versucht mit zwei Clients zu einem nicht nebenläufigen Server eine Verbindung aufzubauen.  
**Versuchsdurchführung**  
Client A wird gestartet und sendet unverzüglich Daten zum Server. Anschließend wird in einem weiteren Fenster, ein zweiter Client B gestartet und Daten zum Server abgeschickt. Danach wird die Übertragung von A fortgesetzt und nach der Terminierung von A wird B fortgesetzt und beendet.  
**Woran ist eindeutig erkennbar, dass der Server sequentiell arbeitet?**  
Daran, dass Client B erst behandelt wird nachdem Client A die Verbindung beendet hat.  
**Wo blockiert der Server?**  
Wird durch die Funktion listen() blockiert, da nur eine Verbindung zugelassen ist.

Three Way Handshake:
```
No.     Time           Source                Destination           Protocol Length Info                                                            Delta TIme
41    0.000000	134.108.8.36	134.108.8.37	TCP	74	48072 → 9001 [SYN] Seq=0 Win=2920 Len=0 MSS=1460 SACK_PERM=1 TSval=8168450 TSecr=0 WS=1
42    0.000177	134.108.8.37	134.108.8.36	TCP	74	9001 → 48072 [SYN, ACK] Seq=0 Ack=1 Win=2896 Len=0 MSS=1460 SACK_PERM=1 TSval=6534371 TSecr=8168450 WS=1
43    0.000023	134.108.8.36	134.108.8.37	TCP	66	48072 → 9001 [ACK] Seq=1 Ack=1 Win=2920 Len=0 TSval=8168450 TSecr=6534371
```

## Aufgabe 1.1.3

**Versuchsziel**  
Ein Server (nicht neben läufig) und ein Client, jedoch wird die Verbindung frühzeitig
von der Clientseite beendet.

**Versuchsdurchführung**  
Der Server und der Client werden gestartet. Im Server beantworten wir
zunächst die Frage nach Lesen mit „n“ und die anschließende Frage nach Schreiben mit „ j“. Im Client
beantworten wir nun die Frage nach Schreiben mit „n“ und die anschließende Frage nach Lesen
ebenfalls mit „n“.

**Warum wird kein PDU mit FIN gesendet?**  
Der Server schickt zwar Daten, welche auch im Buffer landen aber der Client lehnt jede
reinkommende Nachricht vom Server ab. Wie man auch am Schluss sieht wird sogar die Bestätigung
des RST Signals abgelehnt.

**Wozu dient die RST-PDU?**  
Damit der Server mitgeteilt bekommt das der Client nichts annehmen will.

**Was passiert mit den Daten des Servers?**  
Die befinden sich im Buffer werden aber nicht ausgelesen.

```
No.     Time           Source                Destination           Protocol Length Info                                                            Delta TIme
552 55.401718      134.108.8.36          134.108.8.37          TCP      74     48088 → 9001 [SYN] Seq=0 Win=2920 Len=0 MSS=1460 SACK_PERM=1 TSval=8300020 TSecr=0 WS=1 0.047557
553 55.401926      134.108.8.37          134.108.8.36          TCP      74     9001 → 48088 [SYN, ACK] Seq=0 Ack=1 Win=2896 Len=0 MSS=1460 SACK_PERM=1 TSval=6665941 TSecr=8300020 WS=1 0.000208
554 55.401970      134.108.8.36          134.108.8.37          TCP      66     48088 → 9001 [ACK] Seq=1 Ack=1 Win=2920 Len=0 TSval=8300021 TSecr=6665941 0.000044
739 77.220823      134.108.8.36          134.108.36.102        TCP      66     899 → 2049 [ACK] Seq=1369 Ack=1665 Win=501 Len=0 TSval=8321840 TSecr=3162258124 0.007013
757 79.474738      134.108.8.36          134.108.34.11         TCP      112    56612 → 3128 [PSH, ACK] Seq=47 Ack=47 Win=146 Len=46 TSval=8324093 TSecr=335999351 0.687056
758 79.479684      134.108.34.11         134.108.8.36          TCP      112    3128 → 56612 [PSH, ACK] Seq=47 Ack=93 Win=282 Len=46 TSval=336014102 TSecr=8324093 0.004946
759 79.479714      134.108.8.36          134.108.34.11         TCP      66     56612 → 3128 [ACK] Seq=93 Ack=93 Win=146 Len=0 TSval=8324098 TSecr=336014102 0.000030
779 85.167953      134.108.8.36          134.108.8.37          TCP      69     48088 → 9001 [PSH, ACK] Seq=1 Ack=14001 Win=2920 Len=3 TSval=8329787 TSecr=6685743 0.160108
780 85.167983      134.108.8.36          134.108.8.37          TCP      66     48088 → 9001 [RST, ACK] Seq=4 Ack=14001 Win=2920 Len=0 TSval=8329787 TSecr=6685743 0.000030
781 85.168191      134.108.8.37          134.108.8.36          TCP      66     9001 → 48088 [ACK] Seq=14001 Ack=4 Win=2896 Len=0 TSval=6695708 TSecr=8329787 0.000208
782 85.168218      134.108.8.36          134.108.8.37          TCP      54     48088 → 9001 [RST] Seq=4 Win=0 Len=0                          0.000027
```


## Aufgabe 1.2

Filter: tcp.port == 9001 or icmp

**Wie wurde die MSS berechnet?**  
1500 Bytes - 20 Bytes [IP] - 20 Bytes [TCP] = 1460
Da wir aber noch Tunneln, 1460 - 24 Bytes [Tunneling] = 1436 Bytes

**Was sagt die ICMP Nachricht vom Router?**  
Der Router sendet eine ICMP Nachricht, dass eine Fragmentierung notwendig ist.

```
No.     Time           Source                Destination           Protocol Length Info                                                            Delta TIme
183	19.314823	134.108.11.254	134.108.8.36	ICMP	70	Destination unreachable (Fragmentation needed)	0.000623
```

**Warum soll fragmentiert werden?**  
Da Ethernet max. 1500 Bytes pro Paket versenden kann.

**Wie sieht TCP Segmentierung aus? Ist diese Segmentierung sinnvoll?**  
Es wird in zwei Datenpakete fragmentiert, einmal mit 1424 Bytes
und einmal mit 24 Bytes, wodurch unnötig Datenverkehr produziert wird.


**Mit Fragmentierung:**

```
No.     Time           Source                Destination           Protocol Length Info                                                            Delta TIme
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

**Ohne Fragmentierung:**

```
No.     Time           Source                Destination           Protocol Length Info                                                            Delta TIme
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

### Aufgabe 1.3

Da der Client die Verbindung vorzeitig beendet hat, haben wir einen half-closed status.
Der Client sendet [FIN, ACK] und bekommt immernoch Daten, bis der Server ein [ACK] sendet und darauf ein weiteres [FIN, ACK] , [ACK] zum beenden vom Server bekommt.

```
No.     Time           Source                Destination           Protocol Length Info                                                            Delta TIme
127	10.534906	134.108.8.37	134.108.8.36	TCP	74	55002 → 9001 [SYN] Seq=0 Win=2920 Len=0 MSS=1460 SACK_PERM=1 TSval=11309456 TSecr=0 WS=1	0.245141
128	10.534950	134.108.8.36	134.108.8.37	TCP	74	9001 → 55002 [SYN, ACK] Seq=0 Ack=1 Win=2896 Len=0 MSS=1460 SACK_PERM=1 TSval=12943535 TSecr=11309456 WS=1	0.000044
129	10.535157	134.108.8.37	134.108.8.36	TCP	66	55002 → 9001 [ACK] Seq=1 Ack=1 Win=2920 Len=0 TSval=11309456 TSecr=12943535	0.000207
215	20.989058	134.108.8.37	134.108.8.36	TCP	66	55002 → 9001 [FIN, ACK] Seq=1 Ack=1 Win=2920 Len=0 TSval=11319911 TSecr=12943535	0.044454
216	20.989127	134.108.8.36	134.108.8.37	TCP	66	9001 → 55002 [ACK] Seq=1 Ack=2 Win=2896 Len=0 TSval=12953989 TSecr=11319911	0.000069
597	66.007860	134.108.8.36	134.108.8.37	TCP	1514	9001 → 55002 [ACK] Seq=1 Ack=2 Win=2896 Len=1448 TSval=12999008 TSecr=11319911	0.002556
598	66.007902	134.108.8.36	134.108.8.37	TCP	1514	9001 → 55002 [PSH, ACK] Seq=1449 Ack=2 Win=2896 Len=1448 TSval=12999008 TSecr=11319911	0.000042
599	66.008144	134.108.8.37	134.108.8.36	TCP	66	55002 → 9001 [ACK] Seq=2 Ack=2897 Win=2920 Len=0 TSval=11364930 TSecr=12999008	0.000242
600	66.008168	134.108.8.36	134.108.8.37	TCP	1514	9001 → 55002 [ACK] Seq=2897 Ack=2 Win=2896 Len=1448 TSval=12999008 TSecr=11364930	0.000024
601	66.008176	134.108.8.36	134.108.8.37	TCP	1514	9001 → 55002 [PSH, ACK] Seq=4345 Ack=2 Win=2896 Len=1448 TSval=12999008 TSecr=11364930	0.000008
602	66.008360	134.108.8.37	134.108.8.36	TCP	66	55002 → 9001 [ACK] Seq=2 Ack=5793 Win=2920 Len=0 TSval=11364930 TSecr=12999008	0.000184
603	66.008374	134.108.8.36	134.108.8.37	TCP	1514	9001 → 55002 [ACK] Seq=5793 Ack=2 Win=2896 Len=1448 TSval=12999008 TSecr=11364930	0.000014
604	66.008380	134.108.8.36	134.108.8.37	TCP	1514	9001 → 55002 [PSH, ACK] Seq=7241 Ack=2 Win=2896 Len=1448 TSval=12999008 TSecr=11364930	0.000006
605	66.008591	134.108.8.37	134.108.8.36	TCP	66	55002 → 9001 [ACK] Seq=2 Ack=8689 Win=2920 Len=0 TSval=11364930 TSecr=12999008	0.000211
606	66.008603	134.108.8.36	134.108.8.37	TCP	1514	9001 → 55002 [ACK] Seq=8689 Ack=2 Win=2896 Len=1448 TSval=12999008 TSecr=11364930	0.000012
607	66.008608	134.108.8.36	134.108.8.37	TCP	1514	9001 → 55002 [PSH, ACK] Seq=10137 Ack=2 Win=2896 Len=1448 TSval=12999008 TSecr=11364930	0.000005
608	66.008822	134.108.8.37	134.108.8.36	TCP	66	55002 → 9001 [ACK] Seq=2 Ack=11585 Win=2920 Len=0 TSval=11364930 TSecr=12999008	0.000214
609	66.008833	134.108.8.36	134.108.8.37	TCP	1514	9001 → 55002 [ACK] Seq=11585 Ack=2 Win=2896 Len=1448 TSval=12999009 TSecr=11364930	0.000011
610	66.008839	134.108.8.36	134.108.8.37	TCP	1034	9001 → 55002 [PSH, ACK] Seq=13033 Ack=2 Win=2896 Len=968 TSval=12999009 TSecr=11364930	0.000006
611	66.009049	134.108.8.37	134.108.8.36	TCP	66	55002 → 9001 [ACK] Seq=2 Ack=14001 Win=2920 Len=0 TSval=11364931 TSecr=12999009	0.000210
620	67.008075	134.108.8.36	134.108.8.37	TCP	66	9001 → 55002 [FIN, ACK] Seq=14001 Ack=2 Win=2896 Len=0 TSval=13000008 TSecr=11364931	0.148259
621	67.008275	134.108.8.37	134.108.8.36	TCP	66	55002 → 9001 [ACK] Seq=2 Ack=14002 Win=2920 Len=0 TSval=11365930 TSecr=13000008	0.000200
```



## Aufgabe 2.1

**Erklären Sie den Ablauf bei UDP:**  
Bei UDP müssen die Ports mit gesendet werden, da UDP ein verbindugsloses Protokoll ist (kein
SYN,ACK/FIN,ACK) wie bei TCP.

**Was ist anders zu TCP?**
Bei UDP wird eine Checksummenprüfung gemacht, ob das Paket beim empfänger angekommen ist
wird nicht überprüft. Im folgenden sieht man den Datenaustausch über UDP wobei im ersten Paket
die Port Nummern übertragen werden und anschließend die Daten.

```
No.     Time           Source                Destination           Protocol Length Info                                                            Delta TIme
1	 0.000000       134.108.8.36          134.108.8.37          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=0, ID=fd65) [Reassembled in #3] 0.000000
2	 0.000009       134.108.8.36          134.108.8.37          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=1480, ID=fd65) [Reassembled in #3] 0.000009
3	 0.000012       134.108.8.36          134.108.8.37          UDP      82     9005 → 9006 Len=3000                                          0.000003
4	 0.000032       134.108.8.36          134.108.8.37          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=0, ID=fd66) [Reassembled in #6] 0.000020
5	 0.000036       134.108.8.36          134.108.8.37          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=1480, ID=fd66) [Reassembled in #6] 0.000004
6	 0.000038       134.108.8.36          134.108.8.37          UDP      82     9005 → 9006 Len=3000                                          0.000002
7	 8.320396       134.108.8.37          134.108.8.36          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=0, ID=35ff) [Reassembled in #9] 8.320358
8	 8.320413       134.108.8.37          134.108.8.36          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=1480, ID=35ff) [Reassembled in #9] 0.000017
9	 8.320417       134.108.8.37          134.108.8.36          UDP      82     9006 → 9005 Len=3000                                          0.000004
```

## Aufabe 2.2

Hier ist das gleiche zu beobachten. Als erstes werden die Port Nummern übertragen und anschließen
die Daten in diesem Fall fragmentiert.


```
No.     Time           Source                Destination           Protocol Length Info                                                            Delta TIme
74    12.783421      134.108.8.36          134.108.8.37          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=0, ID=fd69) [Reassembled in #76] 0.480848
75    12.783433      134.108.8.36          134.108.8.37          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=1480, ID=fd69) [Reassembled in #76] 0.000012
76    12.783436      134.108.8.36          134.108.8.37          UDP      82     9005 → 9006 Len=3000                                          0.000003
77    12.783458      134.108.8.36          134.108.8.37          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=0, ID=fd6a) [Reassembled in #79] 0.000022
78    12.783461      134.108.8.36          134.108.8.37          IPv4     1514   Fragmented IP protocol (proto=UDP 17, off=1480, ID=fd6a) [Reassembled in #79] 0.000003
79    12.783463      134.108.8.36          134.108.8.37          UDP      82     9005 → 9006 Len=3000                                          0.000002
```

TODO : verbesserung notwenig
