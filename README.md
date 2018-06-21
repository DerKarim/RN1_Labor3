# RECHNERNETZE LABOR 3

## Aufgabe 1.1

### Wichtige Funktionen vom Server

|Beschreibung         |Line |Funktion   |
|---------------------|-----|-----------|
|TCP socket           |95   | socket()  |
|Registrierung        |113  |bind()     |
|Schließt den Socket  |116  |close(ss)  |
|Buffer               |127  |listen()   |
|Verbindungsaufbau    |150  |accept()   |
|Lese Buffer          |219  |read()     |
|Schreibe an Client   |270  |write()    |

### Wichtige Funktionen des Clients

|Beschreibung         |Line |Funktion   |
|---------------------|-----|-----------|
|TCP socket           |112  | socket()  |
|Registrierung        |149  |bind()     |
|Schließt den Socket  |155  |close(ss)  |
|Schreibe an Server   |244  |listen()   |
|Lese Buffer          |262  |read()     |
