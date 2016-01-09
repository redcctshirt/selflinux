.. selflinux documentation master file, created by
   sphinx-quickstart on Wed Dec 23 13:39:08 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Systemverwaltung
================

Elementare Informationen
------------------------

Benutzerinformationen
^^^^^^^^^^^^^^^^^^^^^

Einführung
""""""""""

Die folgenden Beispiele sind unter  X größtenteils nicht funktionsfähig, weil die Kommandos nicht herausfinden können, welcher Benutzer angemeldet ist. Darum ist die Verwendung einer Textkonsole empfehlenswert.

Wenn Sie gerade unter  X arbeiten, können Sie mit der Tastenkombination Strg+Alt+F1 bis Strg+Alt+F6 auf eine Textkonsole wechseln. Hier werden Sie sich nun vermutlich erst anmelden müssen. Geben Sie wie üblich Ihren Benutzernamen und Ihr Passwort ein. Um wieder zu  X zu wechseln genügt Alt+F7.

id
""

id gibt Informationen über die Daten, mit denen die Benutzer eines Linux-Systems verwaltet werden, aus. Jeder Benutzer hat eine Benutzernummer, die sogenannte  UID. Zudem ist er Mitglied in einer oder mehreren Gruppen von Benutzern, was wichtig für die Regelung von Zugriffsrechten und Rechten zur Nutzung von Systemressourcen ist.
user@linux $ id
uid=500(pinguin) gid=100(users) groups=100(users),14(uucp)

Unser Pinguin ist der Benutzer mit der Nummer 500, und seine wichtigste Gruppe ist die Gruppe 100 mit dem Namen users. Auf fast jedem Linux-System gibt es diese Gruppe, und normalerweise sind alle Benutzer Mitglied in ihr.

Er gehört auch noch einer anderen Gruppe an, der Gruppe uucp, die traditionell das Recht gibt, über Modemverbindungen Daten mit der Außenwelt auszutauschen.

id kann auch Informationen über andere Benutzer auf dem System liefern:
user@linux $ id root
uid=0(root) gid=0(root)
groups=0(root),1(bin),14(uucp),15(shadow),16(dialout),17(audio),65534
(nogroup)

Dies ist der Systemverwalter  root. Er hat seine eigene Gruppe.

logname
"""""""

Mit diesem Kommando wird der Benutzername des Benutzers abgefragt, der es aufruft. logname lässt sich auch von Kommandos, die es einem Benutzer ermöglichen, vorübergehend einen anderen Namen anzunehmen, nicht irritieren:
user@linux $ logname
pinguin
user@linux $ su
Kennwort:
root@linux # logname
pinguin
root@linux # exit
user@linux $ logname
pinguin

who
"""

Linux ermöglicht es Benutzern, sich über ein Netzwerk (das kann auch ein Modem sein) am System anzumelden und zu arbeiten. Daher können mehrere Benutzer zur gleichen Zeit angemeldet sein. Das Kommando who gibt einen Überblick über die angemeldeten Benutzer:
user@linux $ who
pinguin        tty1                    Jan 18 22:27
eisbaer        tty3                    Jan 20 07:45

Dies bedeutet, dass der Pinguin auf der ersten Textkonsole angemeldet ist, und der Eisbär auf der dritten. In der dritten Spalte steht der Zeitpunkt der Anmeldung.
user@linux $ who am i
pinguin   tty1           Jan 18 22:27

who am i (Wer bin ich) zeigt nur den Benutzer an, der das Kommando aufgerufen hat. Diese Angaben lassen sich in Shell-Skripten leicht auswerten.

Zwei interessante Optionen sind -H und --login.

    -H (headline) gibt zusätzlich noch eine Kopfzeile mit aus, wie Sie es von den meisten anderen Programmen gewöhnt sind.
    --login gibt nicht die Benutzer, die gerade angemeldet sind aus, sondern zeigt an, wo gerade ein Login-Prozess läuft.


w
"

w ist wesentlich gesprächiger als who. Es gibt alle Informationen, die auch who liefert, und zusätzlich noch statistische Daten über Rechenzeitverbrauch und gerade aktive Programme:
user@linux $ w
  9:49pm  up 14 days,  4:29,  2 users,  load average: 1.08, 1.02, 1.01
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU  WHAT
pinguin  tty1     -                Tue10pm  2days  0.42s  0.31s  -bash
eisbaer  pts/3    sp.antarktis.net  9:49pm  0.00s  0.32s  0.06s  w

Diese Zeilen enthalten:

    Die aktuelle Uhrzeit. (9:49 pm = 21:49 h)
    Die Uptime: Das System läuft seit zwei Wochen, vier Stunden und 29 Minuten. (up 14 days, 4:29)
    Die Anzahl der angemeldeten User: 2 (2 users)
    Die Systemauslastung in der letzten Minute, den letzten fünf Minuten und den letzten 15 Minuten. (load average: 1.08, 1.02, 1.01)
    Zusätzlich zu den von who bekannten Daten in den Spalten USER, TTY und LOGIN@ noch:
    FROM: Den Namen des Computers, von dem aus sich der Benutzer angemeldet hat. Der Pinguin sitzt an dem Computer selbst, der Eisbär könnte irgendwo sein, denn er ist über ein Netzwerk angemeldet.
    IDLE: Die Zeit, seit der jeweilige Benutzer zuletzt etwas getan hat: Pinguin ist schon seit zwei Tagen untätig, er wird wahrscheinlich vergessen haben, sich abzumelden.
    JCPU: Die Rechenzeit, die alle zum gegenwärtigen Zeitpunkt laufenden Programme bisher belegt haben. Hier lässt sich erkennen, ob ein Benutzer ein Programm gestartet hat, das viel Rechenzeit verbraucht. Der Systemverwalter kann dann den Benutzer auffordern, das Programm zu beenden, er kann dem Programm weniger Rechenzeit zuteilen, oder er kann es eigenmächtig beenden.
    WHAT: Das Programm, dass der Benutzer zuletzt gestartet hat, wenn es nicht im Hintergrund abläuft.
    PCPU: Die bisher von dem unter WHAT angegebenen Programm verbrauchte Rechenzeit.


finger
""""""

Um mehr über einen Benutzer zu erfahren, gibt es das Tool finger. Wie viel Informationen finger ausgibt, hängt davon ab, wie viel der Systemverwalter und der Benutzer zulässt. finger sucht unter anderem nach Informationen in den Dateien ~/.plan und ~/.project. Damit finger die Dateien .plan und .project ausgeben kann, muss finger darauf zugreifen können. Da diese Dateien im home-Verzeichnis liegen, ist dieser Umstand nicht immer gegeben.

Ohne Angabe eines Benutzernamens wird eine Kurzinformation über alle angemeldeten Benutzer ausgegeben.
user@linux $ finger
Login   Name    Tty     Idle    Login Time      Office  Office Phone
pinguin                 *tty1   1:45  Jul 26  9:56
eisbaer                 *tty2       7   Jul 26 11:18

    Login: Hiermit ist der Login-Name gemeint.
    Name: Das ist der reale Name des Users. Bei beiden wurde kein Name angegeben.
    Tty: Hier ist das Terminal angegeben, von wo aus sich der Benutzer angemeldet hat.
    Idle: Gibt wie auch bei w die Zeitspanne an seit dem der Benutzer inaktiv ist.
    Login Time: Das ist die Angabe, wann er sich angemeldet hat.
    Office: Gibt Informationen zum Büro, z. B. Adresse aus.
    Office Phone: Hier steht, wenn vorhanden, die Telefonnummer des Büros, in welchem der Benutzer arbeitet.

Gibt man einen Benutzernamen mit an, werden genauere Informationen über diesen einen Benutzer ausgegeben.
user@linux $ finger pinguin
Login: pinguin                          Name: (null)
Directory: /home/pinguin                Shell: /bin/bash
On since Sat Jul 26 11:08 (CEST) on tty2        8 minutes 2 seconds idle
        (messages off)
No mail.
No Plan.

    Login und Name wurden schon weiter oben erklärt.
    Directory gibt das home-Verzeichnis des Benutzers an. In diesem Fall ist es wie für Benutzer üblich /home/pinguin.
    Shell gibt die  Standardshell des Benutzers an. Pinguin benutzt  /bin/bash.
    Danach erfolgt die Angabe, seit wann und wo der Benutzer angemeldet ist, gefolgt von der Idle-Zeit.
    messages off sagt aus, dass der Benutzer keine Nachrichten empfangen kann.
    No mail bedeutet dass der Benutzer keine Mails in seinem Mailordner hat. Wenn ungelesene Mails vorhanden sind, wird zum dem noch angegeben, seit wann die Mails nicht gelesen wurden.
    No Plan sagt aus, dass die Datei .plan nicht vorhanden ist oder finger darauf keinen Zugriff hat.

Informationen über die Speicherbelegung
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

free
""""

Von Zeit zu Zeit ist es nützlich, die Speicherbelegung in Augenschein zu nehmen, beispielsweise weil ein Programm nicht genug Speicher bekommt, oder wenn der Systemverwalter sich für die Auslastung des Auslagerungsbereiches interessiert.
user@linux $ free
             total       used       free   shared     buffers   cached

Mem:        128284(1)  117228(2)  11056(3)  25644(4)  5736(5)   64304(6)

-/+ buffers/cache:      47188(7)   81096(8)

Swap:       128480(9)  13376(10) 115104(11)

(Die Zahl in Klammern dient lediglich der Erklärng und ist keine Ausgabe von free.)

Diese vielen Zahlen bedeuten:

    Die gesamte für das System verfügbare Speichermenge. Hier ist der größte Teil von 128 MB verfügbar, weil der vom Kernel belegte Speicherplatz nicht mitgerechnet wird.
    Die Menge des belegten Speichers.
    Die Menge des freien Speichers.
    Die Größe von zwischen Prozessen geteilten Speicherbereichen.
    Der für verschiedene Arten von Zwischenspeichern (Caches) verwendete Speicher. Der Unterschied ist nicht von Bedeutung, wenn man nicht gerade am Kernel programmiert.
    siehe 5.
    Der belegte Speicher nach Abzug aller Zwischenspeicherarten. Diese Speichermenge ist tatsächlich von Programmen belegt.
    Der freie Speicher nach Abzug aller Zwischenspeicherarten. Dieser Speicher ist noch für Programme verfügbar.
    Die Gesamtgröße der vorhandenen Auslagerungsbereiche. Hier sind es knapp 128 MB.
    Der genutzte Teil des Auslagerungsbereiches. Untätige Prozesse werden in den Auslagerungsbereich verschoben, wenn der von ihnen belegte Arbeitsspeicher besser für andere Zwecke verwendet werden kann.
    Der freie Teil des Auslagerungsbereiches.

Mit den Optionen -b, -k und -m wird der Speicher in Byte, KByte, bzw. in MByte ausgegeben. Als Standard-Einstellung wird KByte verwendet.

Die Option -t (total) gibt zusätzlich noch die Summe der Gesamtgrößen aus.

df
""

Eines der häufigsten Probleme beim Betrieb eines Linux-Systems, und noch dazu eines, das sich oft nicht rechtzeitig zu erkennen gibt, ist eine vollgelaufene Festplatte. Heutige Linux-Distributionen enthalten derartige Mengen an Software, dass es überhaupt kein Problem ist, auch eine Fünf- oder mehr Gigabyte-Partition in kürzester Zeit zu füllen.

df zeigt den Belegungszustand jedes eingehängten Dateisystems an.
user@linux $ df
Filesystem           1k-blocks      Used Available Use% Mounted on
/dev/sda2               208820     76912    131908  37% /
/dev/sda5               763012    104468    658544  14% /var
/dev/sda3              5245048   3128612   2116436  60% /usr
/dev/sda1                 7988      2404      5184  32% /boot
ser1:/home/pinguin     2097286   1305429    686633  66% /home/pinguin

In der ersten Spalte (Filesystem) steht die Bezeichnung des Dateisystems, meistens eine Festplattenpartition. Dahinter steht die Gesamtgröße in Kilobyte (1k-blocks), der belegte Speicher (Used) und der noch vorhandene freie Platz (Available). Außerdem noch der Anteil des belegten Speichers an der Gesamtgröße (Use%) und das Verzeichnis, in das das Dateisystem eingehängt ist (Mounted on).

In der letzten Zeile ist ein Dateisystem zu sehen, auf das über das Netzwerk zugegriffen wird.

Mit der Option -a werden auch Dateisystem angezeigt, die eine Kapazität von 0 Byte haben. Damit sind Dateisysteme gemeint, die im Prinzip keinen Speicherplatz belegen, sondern eine bestimmte Funktionalität zur Verfügung stellen. Ein Beispiel ist das devfs-Dateisystem, welches nur Dateien beinhaltet, die Geräte darstellen.

Wenn man noch zu jedem Dateisystem dem Typ erfahren will, so gibt es dafür noch die Option -T.

Die Option -i zeigt anstelle der Speicherbelegung die Belegung der Inodes an. Unter Linux benötigt jede Datei eine bestimmte Inode-Nummer. Es gibt allerdings immer eine maximale Anzahl an Inode-Nummern pro Dateisystem. Wenn diese Anzahl erreicht ist, kann keine weitere Datei mehr angelegt werden, egal wie viel Speicher noch frei ist.

Wie Sie im Beispiel sehen konnten, sind die Zahlen zum Teil ziemlich unhandlich und somit weniger aussagekräftig. Aus diesem Grund gibt es die Optionen -h, bzw. -H (human-readable). Diese haben die Aufgabe, die Zahlen für den Menschen besser lesbar darzustellen. Der Unterschied zwischen den beiden Optionen besteht darin, dass -h mit einer Potenz von 1024 und -H von 1000 rechnet. Zudem stehen noch die Optionen -k und -m zur Verfügung. Diese geben den Plattenplatz in Kilobyte, bzw. in Megabyte aus. Mit --block-size=n erfolgt die Ausgabe in n-Byte-Blöcken.

du
""

Mit Hilfe von du (disk usage) wird der Speicherplatzverbrauch für ein Verzeichnis und dessen Unterverzeichnisse angezeigt. Als Standard-Einstellung wird das aktuelle Arbeitsverzeichnis verwendet.
user@linux $ du
...
120             ./.kde/share/apps/kMail
...

Beim Autor war die Ausgabe von du wesentlich länger. Diese eine Zeile soll exemplarisch betrachtet werden.

Das Beispiel sagt aus, dass das Verzeichnis ./.kde/share/apps/kMail insgesamt 120 Kilobyte belegt (als Standard-Einstellung erfolgt die Ausgabe in Kilobytes). Um die Ausgabe besser lesbar darzustellen, gibt es wie bei df die Optionen -h und -H.

Ebenfalls stehen wie bei df die Optionen -k und -m für eine Ausgabe in Kilo- bzw. Megabyte zur Verfügung. Hinzu kommt noch die Option -b für Byte.

Um nur die Gesamtsumme, die ein bestimmtes Verzeichnis belegt, zu erfahren, verwendet man die Option -s.
user@linux $ du -s
40248   .

Auf den ersten Blick mag diese Ausgabe ein wenig ungewohnt erscheinen, haben wir doch erwartet, dass alle Verzeichnisse im aktuellem Verzeichnis ausgegeben werden. Bei genauerer Betrachtung ist die Ausgabe aber durchaus logisch. Als Standard-Einstellung wird das aktuelle Arbeitsverzeichnis genommen und das ist nun mal ./".

Will man alle Unterverzeichnisse aufgelistet haben, so muss der Befehl du -s * lauten.

Um zusätzlich noch den belegten Speicher für jede Datei auszugeben, gibt es die Optionen -a, bzw. --all.

Die Option -c gibt am Ende noch die Gesamtsumme aus. 

Weitere Kommandos
^^^^^^^^^^^^^^^^^

dmesg
"""""

Der Kernel gibt im Laufe der Zeit eine Menge Informationen an den  klogd, den kernel log daemon weiter. dmesg zeigt die aktuellsten Meldungen an. Hier sind z. B. die Bootmeldungen nachzulesen, aber auch das Einlegen einer neuen CD wird hier vermerkt. Weil in diesen Meldungen auch all die Hardwareinformationen enthalten sind, die beim Hochfahren des Systems anfallen, ist dmesg für die Fehlersuche oder die Konfiguration sehr nützlich.

date
""""

Wer möchte nicht gerne wissen, welcher Tag heute ist (vor allem nach stundenlangem Programmieren, vorzugsweise nachts). Dazu gibt date das aktuelle Datum und die Uhrzeit aus:
user@linux $ date
Fr Jan 21 22:57:58 CET 2000

Manchmal kann es aber notwendig sein, sich das Datum in einem individuellem Format ausgeben zu lassen. Wenn dies der Fall ist, sieht der Befehl folgendermaßen aus:

date +'Format'

Format ist eine Zeichenfolge, die angibt, wie das ausgegebene Datum aussehen soll. Dabei stehen dem Benutzer verschiede Platzhalter, die durch aktuelle Werte ersetzt werden, zur Verfügung. Die wichtigsten sind unter anderem:

    %d: Aktueller Tag des Monats von 01 bis 31
    %H: Aktuelle Stunde von 00 bis 23
    %m: Aktueller Monat von 01 bis 12
    %M: Aktuelle Minute von 00 bis 59
    %Y: Aktuelles Jahr von 1970 bis ...

Beispiel:
user@linux $ date +'%Y-%m-%d'
2000-01-21

Von Zeit zu Zeit will man sich aber auch das Datum in einem bestimmten standarisiertem Format ausgeben lassen. Dazu stehen folgen Optionen zur Verfügung:

    -I[TIMESPEC], --iso-8601[=TIMESPEC]
    Gibt das Datum im ISO 8601 Format aus. Als TIMESPEC kann date (gibt nur das Datum aus), hours (gibt zudem noch die Stunde an), minutes (die Minuten werden noch zusätzlich angezeigt) oder seconds (die Sekunden werden auch noch angezeigt) sein. Als Default für TIMESPEC ist date eingestellt.
    -R, --rfc-822
    Gibt das Datum im RFC 822 Format aus.
    -u, --utc, --universal
    Gibt das Datum im UTC Format aus.

Besitzt man die nötigen administrativen Rechte, kann man mit date auch Datum und Uhrzeit ändern. Dazu gibt es die Option -s. Wichtig dabei ist dennoch ein Format anzugeben. Das Format gibt dann an in welcher Form das Datum übergeben wird. Die genaue Syntax lautet wie folgt:

date +'Format' -s "Datum"

bzw.

date +'Format' --set="Datum"

Dabei gibt 'Datum' das neue Datum an. Selbstverständlich kann anstelle von +'Format' auch eine der Optionen -I, -R oder -u verwendet werden.

dd
""

Die primäre Aufgabe von dd ist es Daten zu kopieren und entsprechend zu konvertieren. Dabei kopiert dd die Daten nicht Dateiweise, sondern Blockweise. Das Haupteinsatzgebiet von dd ist es Kopien von ganzen Devices auf einem anderen (physikalischen) Device anzulegen. Somit eignet sich dd auch dafür, ein ISO-Abbild von einer CD auf der Festplatte zu sichern.

dd besitzt eine Reihe von verschiedenen Optionen, die verschiedene Möglichkeiten der Konvertierung darstellen. Die beiden wichtigsten Optionen sind allerdings if=DATEI und of=DATEI. if=DATEI gibt dabei die Datei an, von der gelesen werden soll. of=DATEI gibt die Datei an, in die geschrieben werden soll.

Ein Beispiel, um ein ISO-Abbild auf der Festplatte im aktuellen Verzeichnis anzulegen, lautet:
root@linux # dd if=/dev/cdrom of=cdrom.iso

Wie schon angesprochen kopiert dd nicht Dateiweise sondern Blockweise. Aus diesem Grund ist es auch möglich Daten zu kopieren und dabei die Blockgröße zu ändern. Ebenso ist es auch möglich nur bestimmte Blöcke von einem Device zu kopieren.

Um die Blockgröße für die Ein- und Ausgabe festzulegen, stehen drei Optionen zur Verfügung:

    ibs=n: Legt die Eingabeblockgröße auf n Bytes fest.
    obs=n: Legt die Ausgabeblockgröße auf n Bytes fest.
    bs=n: Legt die Ein- und Ausgabeblockgröße auf n Bytes fest. bs hat Vorrang von ibs und obs.
    cbs=n: Legt die Datensatzlänge auf n Bytes fest.

Die voreingestellte Größe für die Ein- und Ausgabeblöcke ist 512 Byte.

Um nur bestimmte Blöcke zu kopieren, stehen folgende Optionen zur Verfügung:

    skip=n: Überspringt n-Blöcke am Anfang der Eingabedatei.
    seek=n: Überspringt n-Blöcke am Anfang der Ausgabedatei
    count=n: Kopiert nur n Eingabeblöcke.

Als Blockgröße wird logischerweise, der durch bs, ibs, bzw. obs angegeben Wert genutzt.

Eine weitere wichtige Option ist conv=Key. Wenn diese Option angegeben ist, wird die Eingabedatei entsprechend durch das mit Key angegeben Schlüsselwort in die Ausgabedatei konvertiert. Als Key können auch mehrere Schlüsselwörter, durch Kommata getrennt angeben werden.

Unter anderem stehen dem Benutzer dabei folgenden Schlüsselwörter zur Verfügung:

    block/unblock
    Es gibt Datensätze fester Länge und es gibt Datensätze mit variabler Länge, deren Ende durch einen Zeilenumbruch markiert ist. Das Schlüsselwort block füllt einen Datensatz, der kleiner als cbs-Bytes ist mit Leerzeilen auf, bis die entsprechende Datensatzlänge erreicht ist. Somit wandelt block Datensätze variabler Länge in Datensätze mit fester Länge um. Entsprechend entfernt unblock die nachfolgenden Leerzeilen und wandelt somit Datensätze mit fester Länge in Datensätze mit variabler Länge um.
    lcase
    Sämtliche Großbuchstaben werden in Kleinbuchstaben umgewandelt
    ucase
    Sämtliche Kleinbuchstaben werden in Großbuchstaben umgewandelt
    noerror
    Die Verarbeitung wird auch nach einem Fehler fortgesetzt.


which
"""""

Wenn Sie auf der Shell einen Befehl eingeben, werden der Reihe nach alle Verzeichnisse in $PATH nach diesem Befehl durchsucht. Nachdem der Befehl in einem Verzeichnis gefunden wurde, wird die Suche abgebrochen. Dabei kann allerdings das Problem auftreten, dass Sie ein anderes Kommando meinen, das sich in einem anderen Verzeichnis befindet. Um heraus zufinden, wo sich nun das Programm befindet, das ausgeführt wird, gibt es das Werkzeug which.

which durchsucht alle Verzeichnisse, die in der Umgebungsvariablen $PATH aufgelistet sind, nach einer ausführbaren Datei mit dem angegebenen Namen:
user@linux $ which man
/usr/bin/man

Auf diese Weise finden Sie heraus, ob das seltsame Verhalten eines Kommandos dadurch verursacht wird, dass ein anderes Programm in ein Verzeichnis geraten ist, das sich im Pfad weiter vorne befindet:
user@linux $ man ls
cat: ls: No such file or directory
user@linux $ which man
/usr/local/bin/man

Hier ist ein Programm namens  man in dem Verzeichnis /usr/local/bin, das im Suchpfad vor dem Verzeichnis /usr/bin steht, in dem sich das gewünschte Kommando  man befindet. Durch den Aufruf:
user@linux $ /usr/bin/man ls

können Sie jetzt das richtige Programm aufrufen.

which" kann zudem nützlich sein, um festzustellen, ob ein bestimmtes Programm vorhanden ist ohne dieses Programm gleich ausführen zu müssen. 

Bearbeitung von Programmausgaben
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Manchmal können Ausgaben von Programmen recht umfangreich werden. Um die Ausgaben ein wenig übersichtlicher zu gestalten, stehen dem Benutzer dieselben Werkzeuge mit denselben Optionen und derselben Funktionalität zur Verfügung wie bei der Dateiverwaltung, also z. B. less, grep usw. Dabei muss man die Ausgabe von dem einem Programm per  Pipe in ein anderes Programm weiterleiteten, z. B.:
user@linux $ ps -ax | less

Der Befehl ps -ax" wird ganz normal ausgeführt, aber die Ausgabe dient in diesem Fall als Eingabe für  less. Aus diesem Grund muss auch kein Dateiname für less angeben werden, da die Ausgabe von ps -ax von less wie eine Datei behandelt wird. Das Resultat dieser Kombination ist, dass man durch die gesamte Ausgabe von ps -ax bewegen kann und man sich nicht nur mit den letzten Zeilen zu Frieden geben muss.

Ausführliche Informationen zu ps findet man unter  ps. 

Autoren

    Frank Börner f.boerner@ngi.de
    Ferdinand Hahmann FerdinandHahmann@gmx.net
	
Formatierung

    Florian Frank florian.frank@pingos.org
    Matthias Hagedorn matthias.hagedorn@selflinux.org

Prozessverwaltung
-----------------

Einleitung
^^^^^^^^^^

Linux ist ein de Multitasking-Betriebssystem. Das heißt, es können mehrere Programme zur selben Zeit ablaufen. Üblicherweise benutzt jeder Anwender ein Programm im Vordergrund, während mögliche weitere im Hintergrund ablaufen. Es ist ein Mechanismus vorhanden, um Hintergrundprozesse zu beeinflussen, sie beispielsweise anzuhalten, fortzusetzen oder zu beenden. Ebenso unterstützen die meisten  Shells Job-Control, also die Möglichkeit, Vordergrundprozesse in den Hintergrund zu schicken und umgekehrt. 

ps
^^

In einem Multitasking-Betriebssystem wie  Linux läuft zu jedem Zeitpunkt eine Vielzahl von Prozessen, über die es interessante Dinge herauszufinden gibt. Dazu gibt es das Kommando ps:
user@linux $ ps
  PID TTY      TIME CMD
18301 pts/1    0:00 bash
18901 pts/1    0:00 ps

Die Ausgabe bedeutet, dass unser Pinguin momentan zwei Prozesse betreibt: Eine  Shell und ps selbst.

Die einzelnen Spalten bedeuten folgendes:

    Die PID (Prozess ID) ist eine Nummer, die jeden Prozess eindeutig identifiziert.
    Unter TTY wird der Name des Terminals angegeben, das den Prozess kontrolliert. Meist bedeutet dieser Name, dass der Prozess von diesem Terminal aus gestartet worden ist, und für die Ein- und Ausgabe benutzt wird.
    TIME zeigt die von jedem dieser Prozesse bisher genutzte Rechenzeit an.
    CMD (oder COMMAND) zeigt den Befehl an, mit dem der Prozess gestartet worden ist.

ps hat eine Unmenge von Optionen, die alle dazu dienen, die Berge von Daten, die sich über Prozesse gewinnen lassen, zu filtern. Die einfachste Möglichkeit der Auflistung aller laufenden Prozesse ist ps -ax. Es werden alle Prozesse aufgelistet, auch diejenigen, die nicht vom aktuellen Benutzer gestartet wurden und die, die nicht mit einem Terminal verbunden sind (also im Hintergrund laufen). Diese Liste ist normalerweise sehr lang, daher sind nur die ersten paar Zeilen wiedergegeben.
user@linux $ ps -ax
PID     TTY             STATE   TIME    COMMAND
1       ?               S       0:04    init [2]
2       ?               SW      0:00    [keventd]
3       ?               SWN     0:00    [ksoftirqd_CPU0]
...

Bei ps -ax wird auch noch der Status (STATE) des jeweiligen Prozesses ausgegeben. Dabei werden folgen Abkürzungen verwendet:

    D (Deep sleeping)
    Der Prozess befindet sich in einem ununterbrechbarer Schlaf.
    R (Running)
    Der Prozess verarbeitet im Moment gerade Daten.
    S (Sleeping)
    Der Prozess wartet auf irgendein Ereignis um Daten zu verarbeiten.
    T (Traced)
    Der Prozess wurde angehalten.
    Z (Zombie)
    Zombies sind Prozesse, die eigentlich schon beendet sein sollten, aber immer noch auf irgendetwas warten. Sie können immer mal wieder vorkommen und stellen keinen Grund zur Besorgnis dar, solange sie nicht in Mengen auftreten und es sich nicht um viele Zombies mit gleichem Namen handelt.

Stehen neben diesen Statusmeldungen noch ein oder mehrere Zeichen bedeuten diese folgendes:

    W
    bedeutet, dass der Prozess in den Swap-Speicher ausgelagert wurde und somit keine Speicherseiten belegt werden.
    <
    bedeutet, dass der Prozess mit einer höheren Priorität läuft.
    N
    bedeutet genau das Gegenteil. Der Prozess läuft mit einer niedrigeren Priorität.
    L (Locked)
    bedeutet, dass der Prozess im Speicher geladen wurde und auch dort gehalten wird.


pstree
^^^^^^

Linux merkt sich, wenn ein Programm ein anderes startet. Wenn der Elternprozess beendet wird, wird auch für das Ende aller Kindprozesse gesorgt, damit diese nicht für immer herumliegen und Platz wegnehmen. Die Baumstruktur der laufenden Prozesse wird mit pstree oder mit  ps und der Option -f angezeigt:
user@linux $ pstree
init-+-atd
     |-automount
     |-cron
     |-httpd---httpd
     |-inetd
     |-kbgndwm
     |-kblankscrn.kss
     |-kdm-+-X
     |     `-kdm---kwm
     |-kflushd

     |-kfm-+-Eterm---bash---pstree
     |     |-netscape---netscape
     |     `-xemacs
...

Hier ist ein umfangreicher Ast. Man sieht, wie der Autor an diesem Text mit  XEmacs unter de KDE schreibt. Gleichzeitig laufen noch ein Terminalfenster (Eterm), in dem gerade pstree ausprobiert wird und netscape.

top
^^^

Der Systemverwalter kann seine Augen und Ohren nicht überall haben, darum braucht er ein Programm, das ihm die wichtigsten Prozesse anzeigt, die auf dem System laufen. Die wichtigsten sind die, die am meisten Rechenzeit oder am meisten Speicher belegen. Nach diesen Kriterien entscheidet top.

.. image:: images/prozessverwaltung_prozessverwaltung-top.png

top

top fasst alle bisher genannten Kommandos zusammen und fügt noch einiges hinzu.

    Ganz oben stehen die von uptime bekannten Werte (Systemzeit, Uptime, angemeldete Benutzer und Auslastung).
    In der nächsten Zeile kommt die Anzahl der laufenden Prozesse und ihre Zustände. Die Zustände wurden bereits bei  ps erklärt.
    Die dritte Zeile zeigt den prozentualen Anteil an, womit sich der (oder die) Prozessor(en) in den letzten Sekunden beschäftigt hat (haben).
    user:
    Mit user sind Prozesse gemeint, die von den Benutzern des Systems gestartet worden sind.
    system:
    Hierbei sind sämtliche Prozesse gemeint, die vom System gestartet worden sind.
    nice:
    Damit sind all die Prozesse gemeint, die nicht mit einem  nice von 0 (null) laufen.
    idle:
    Mit idle wird die noch zur Verfügung stehende Prozessorlast bezeichnet.
    Nun kommen noch die von  free bekannten Speicherangaben.

Als letztes folgt die Liste mit den ressourcenträchtigesten Prozessen. Im Gegensatz zu ps werden bei top weitere Informationen zu den einzelnen Prozessen ausgegeben:

    User
    gibt den Namen des Users an, unter welchem der Prozess läuft.
    PR
    gibt die Priorität des Prozesses an
    NI
    gibt den Nice-Wert an. Ein negativer Wert bedeutet eine höhere Priorität, ein positiver Wert eine geringere Priorität.
    VIRT
    gibt die gesamte Anzahl an Speicher an, den dieser Prozess benötigt, inklusive den Code, den Daten und den geteilten Bibliotheken.
    RES
    gibt die Belegung des physikalischen Arbeitsspeichers an. Auslagerungen im Swap-Speicher werden hier nicht berücksichtigt.
    SHR
    gibt die Anzahl des Speichers an Daten an, die auch von anderen Prozessen genutzt werden können.
    S
    gibt den Status des Prozesses an. Siehe dazu auch die Erklärung bei  ps.
    %CPU
    gibt an wieviel Prozent von der gesamten Prozessorleistung der Prozess benötigt.
    %MEM
    gibt dasselbe wie %CPU an, diesmal aber bezogen auf den physikalischen Arbeitsspeicher.


tload
^^^^^

Die Aufgabe von tload ist es, die durchschnittliche Systemlast in einer einfachen ASCII-Grafik darzustellen. Mit der Option -d kann die Zeit in Sekunden angeben werden, nach der die Grafik aktualisiert wird. Mit -s wird die Skalierung angegeben, also wie viele Zeilen für eine Einheit verwendet werden. Je größer die Zahl ist, desto gröber die Darstellung.

fuser
^^^^^

Manchmal kann es ganz nützlich sein, festzustellen welche Prozesse eine bestimmte Datei benutzen. Für diese Aufgabe gibt es den Befehl fuser gefolgt vom Dateinamen.
user@linux $ fuser ksycoca
ksycoca:        463 463m 471 471m 539 539m 546 546m

Diese Ausgabe sagt aus, dass die Datei ksycoca von den Prozessen mit der PID 463, 471, 539 und 546 benutzt wird. Gefolgt von der PID wird ein Buchstabe mit ausgegeben (in diesem Fall ist es immer ein m). Dieser Buchstabe gibt an, wie der Prozess auf die Datei zugreift. Konkret bedeuten die Buchstaben folgendes:

    c:
    Die Datei wird vom Prozess als Verzeichnis behandelt.
    e:
    Die Datei ist ausführbar und wird vom Prozess ausgeführt.
    f:
    Die Datei wurde vom Prozess geöffnet. Im Standard-Modus wird das f weg gelassen. Somit ist auch klar, warum in unserem Beispiel jede PID zweimal angegeben wurde.
    m:
    Die Datei ist eine Bibliothek, die gemeinsam von den Prozessen genutzt wird.
    r:
    wird verwendet, wenn es sich um das  Wurzelverzeichnis (root-Verzeichnis) handelt.

Auch für fuser gibt es noch eine Reihe weiterer Optionen. Interessant dabei ist die Option -n. Diese erwartet noch eine weitere Angabe:

    file,
    wenn es sich um eine Datei handelt. Dabei bewirkt fuser -n file ksycoca dasselbe wie fuser ksycoca
    udp,
    wenn sich die darauf folgende Angabe um einen  UDP-Port handelt.
    tcp,
    bedeutet das gleiche wie udp mit dem Unterschied, dass ein  TCP-Port gemeint ist.

Der oben beschriebe Buchstaben-Code ist auch für die Port-Angaben gültig, da unter Linux Ports auch als Dateien behandelt werden.

Weitere wichtige Optionen sind noch:

    -a:
    Dabei werden auch Dateien ausgegeben, auf die gerade kein Prozess zugreift.
    -u:
    Gibt in Klammern den zur PID gehörenden Benutzernamen aus.
    -v (verbose)
    gibt mehr Informationen aus.


kill
^^^^

kill ist - trotz seines Namens - nicht nur zum Beenden von Prozessen geeignet. Es kann alle möglichen Signale an die Prozesse senden, die vom Benutzer gestartet wurden, der kill aufruft. Ist dieser Benutzer der  Systemverwalter, dann sind ihm alle Prozesse zugänglich.

Der Aufruf ist:

kill <Signal> <Prozess-ID>

Die Prozess-ID (PID) können Sie beispielsweise aus der Ausgabe des Programmes  ps entnehmen:
user@linux $ ps
[...]
19376 pts/0    00:00:00 ps

Hier ist die PID des ps-Prozesses selbst die 19376.

Wenn die PID negativ ist, wird <Signal> nicht an einen bestimmten Prozess gesendet, sondern an alle Prozesse mit der angegebenen PGID. Der Befehl lautet also dann folgendermaßen:

kill <Signal> -<PGID>

Anstelle der Prozess-ID kann auch -1 angegeben werden. Dabei wird <Signal> an alle Prozesse gesendet, ausser dem kill-Prozess selbst und init. Zudem zeigt dieser Befehl nur bei den Prozessen Wirkung, bei welchen man die nötigen Rechte hat.

Wenn Sie kein Signal angeben, sendet kill das Signal 15 (SIGTERM), das die meisten Programme dazu veranlasst, hinter sich aufzuräumen, um sich dann zu beenden.

Weitere wichtige Signale sind SIGHUP (1) und SIGKILL (9). SIGHUP steht für hang-up. Wenn ein Benutzer sich über das Netzwerk angemeldet hat und die Verbindung abbricht, z. B. weil ein Modem auflegt (eben ein hang-up), wird dieses Signal an alle Prozesse gesendet, die während dieser Anmeldesitzung gestartet wurden. Sie werden dann beendet.

SIGKILL tut genau das, was sein Name andeutet: Es unternimmt alles, um einem Prozess den Garaus zu machen.
user@linux $ kill -SIGKILL 12345

Eine Liste mit allen Signalen finden Sie entweder in der  Manpage von kill (man kill) oder mit dem Befehl kill -l.

Die Option -p sendet kein Signal an den Prozess, sondern gibt nur den Namen des zur PID passenden Prozesses aus.

Die  bash-Shell enthält einen eingebauten Befehl kill, der im Prinzip dieselbe Auswirkung wie /bin/kill hat, jedoch zwei Vorteile bietet:

Er erlaubt die Verwendung von Job-IDs statt PIDs, und wenn Sie die maximale Anzahl von Prozessen gestartet haben, die Ihr Systemverwalter ihnen zubilligt, können Sie einen davon beenden, ohne dazu einen weiteren Prozess mit /bin/kill starten zu müssen. Die Eingabe von kill startet den in die  bash eingebauten kill-Befehl. /bin/kill verwendet den externen kill-Befehl.

killall
^^^^^^^

killall bewirkt im Prinzip dasselbe wie  kill. Es sendet Signale an Prozesse. Der Unterschied zwischen kill und killall ist, an welchen Prozesse ein Signal gesendet wird. Bei killall wird nicht die PID des Prozesses angegeben, sondern dessen Name. Da allerdings der Name, im Gegensatz zur PID, nicht eindeutig ist, wird das Signal an alle Prozesse mit diesem Namen gesendet. Der genaue Aufruf lautet:

killall [Option] [Signal] <Name>

Wenn <Name> einen / enthält so ist damit nicht der Name eines Prozesses gemeint, sondern der Name einer auszuführenden Datei. Dabei wird ein Signal an alle Prozesse gesendet, die diese Datei ausführen.

Die Signale sind dieselben wie bei  kill. Ohne Angabe von Signal wird auch ein SIGTERM (15) gesendet.

Eine wichtige Option ist -e (--exact). Normalerweise wertet killall nur die ersten 15 Zeichen von <Name> aus. Haben nun zwei Prozesse unterschiedliche Namen stimmen aber trotzdem in den ersten 15 Zeichen überein, so sind dennoch beide Prozesse von killall betroffen. Die Option -e erzwingt dass der volle Name ausgewertet wird.

Eventuell will man nicht blind alle Prozesse mit gleichen Namen beenden, sondern vorher noch eine Nachfrage haben. Dafür gibt es die Option -i (--interactive). 

nice
^^^^

Unter Linux ist es möglich, dass mehrere Befehle, auch Jobs genannt, zur gleichen Zeit ausgeführt werden. Da allerdings nicht alle Ressourcen unbegrenzt zur Verfügung stehen, muss eine Auswahl getroffen werden, welche Priorität ein Job hat. Diese Aufgabe erledigt der Befehl nice:

nice <Priorität> <Befehl> [Argumente]

<Priorität> ist für normale User eine Zahl zwischen 0 und 19, wobei 19 die niedrigste Priorität ist. Der Superuser hat die Möglichkeit Prioritäten von -20 bis 19 zu vergeben. Dabei ist -20 die höchste Priorität.

<Befehl> ist der eigentliche Befehl, der ausgeführt werden soll. [Argumente] sind dabei Optionen, die an <Befehl> übergeben werden. 

Autor

    Ferdinand Hahmann FerdinandHahmann@gmx.net

Formatierung

    Matthias Hagedorn matthias.hagedorn@selflinux.org


Software-Installation
---------------------

Installieren und Deinstallieren von Software
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Leider gibt es noch kein einheitliches Installations-Verfahren unter Linux, aber mit dem RPM (Redhat Package Manager}-Format von RedHat hat sich mittlerweile ein Format durchgesetzt, das auch von anderen Distributoren verwendet wird. Daneben gibt es noch andere Formate und Verfahren, von denen die Gebräuchlichsten hier vorgestellt werden.

Eines kennzeichnet aber sämtliche Installationsverfahren: kein Reboot nach erfolgter Installation. Sämtliche Tools können sofort gestartet werden, evtl. ist das Starten eines Dienstes (Unix-Jargon: Daemon) nötig, was von der Kommandozeile aus erfolgt.

Linux-Distributoren
^^^^^^^^^^^^^^^^^^^

Die meisten Linux-Distributionen sind recht umfangreich und enthalten bereits die nötigen Tools und Programme, die bei der Grund-Installation ausgewählt wurden. Will man später das eine oder andere Programm nachträglich installieren, kann dies mit den Distributions-eigenen Werkzeugen erfolgen. Auch die Deinstallation ist über diesen Weg möglich. Voraussetzung dafür ist, dass das gewünschte Programm im Distributions-Umfang mit dabei ist.

Leider liegen manche Programme nicht in der neuesten Version vor, andere Programme fehlen, weil beispielsweise die Lizenz des Herstellers nicht mit der Distribution vereinbar ist. Dann muss man sich selbst darum kümmern, an die aktuelle Version des gewünschten Paketes zu kommen, um diese auf dem Rechner installieren zu können. 

RPM
^^^

Das RPM-Format, das von RedHat für ihre Distribution entwickelt wurde, enthält zusammen mit einigen Verwaltungsdaten das compilierte Programm-Paket. Erkennbar sind RPM-Dateien an der Endung .rpm, wobei zusätzlich die Architektur (z. B. i386 oder alpha) im Namen der Datei enthalten ist. So kennzeichnet

kaffe-1.0.6-2.i386.rpm

das Kaffe-Paket für die Intel386-Architektur. Pakete, die nicht an eine bestimmte Architektur gebunden sind (z. B. manche Java-Pakete) erhalten die Endung .noarch.rpm. Handelt es sich um ein Paket in Source-Form, so wird dies durch .src.rpm gekennzeichnet.

Folgende Eigenschaften kennzeichnen das RPM-Format:

    Prüfung, ob die Voraussetzung für ein Paket vorhanden ist
    lokale Installation
    Installation per FTP möglich
    Deinstallation

Wer über FTP installieren will, kann als Paket-Name eine URL angeben, z. B.
user@linux ~$ rpm -ih ftp://ftp.redhat.com/pub/redhat/i386/RedHat/RPMS/kaffe-1.0.6-2.i386.rpm

Das Schöne an der Installation per FTP ist, dass die Abhängigkeiten vor der eigentlichen Installation überprüft werden, d. h. das restliche Paket wird erst heruntergeladen, wenn die Abhängigkeiten erfüllt sind. Dazu teilt sich der eigentliche Installations-Vorgang in drei Phasen auf:

    das Pre-Install-Skript wird ausgeführt (falls vorhanden)
    das eigentliche Archiv wird ausgepackt und in das Dateisystem kopiert
    das Post-Install-Skript wird ausgeführt (falls vorhanden)

Ein ähnliches Schema wird bei der Deinstallation angewandt, auch hier gibt es häufig ein Pre-Uninstall- und Post-Uninstall-Skript.

Andere Distributoren, wie z. B. SuSE oder Mandrake, sind mittlerweile auch auf den RPM-Zug aufgesprungen, so dass dieses Format recht häufig im Internet anzutreffen ist. Allerdings kann man nicht einfach ein SuSE rpm unter Mandrake installieren oder umgekehrt, da die Pakete von den verschiedenen Distributoren teilweise unterschiedlich zusammengebaut werden.

Mit rpm kann man Pakete einzeln, aber auch mehrere auf einmal installieren, erneuern oder entfernen. Sind Pakete dabei, die voneinander abhängig sind, sortiert sie rpm in der richtigen Reihenfolge für die Installation. Dies bedeutet eine erhebliche Erleichterung für den Administrator, da er sich keine Gedanken darüber zu machen braucht, welche Pakete er zuerst installieren muss -- er gibt einfach alle in Frage kommenden Pakete an.
Kommando 	Kurzbeschreibung
rpm -ih x.rpm 	Installation;
die Option -h (oder auch -vh) gibt zusätzlich noch einen Fortschrittsbalken aus
rpm -U x.rpm 	Update;
werden Konfigurationsdaten verändert, werden sie vorher unter der Endung .rpmsave gesichert.
Alternativ wird die neue Version einer Konfigurationsdatei mit der Endung .rpmnew angelegt.
Während des Updates macht der RedHat Package Manager auf diese Aktionen aufmerksam.
rpm -qa 	Query -- Abfrage aller Pakete;
ohne die Option -a kann man gezielt nach einem Paket nachfragen (z.B. rpm -q fileutils)
Hilfreich ist auch die Option -f, mit der man abfragen kann, zu welchem Paket eine Datei (z. B. /bin/ls) gehört.
rpm -e x.rpm 	Erase -- zum Deinstallieren eines Paketes
rmp -V x 	Verify -- ist das Paket noch ordnungsgemäß installiert oder hat da etwa jemand dran manipuliert?

Die  Manual-Page von rpm ist recht umfangreich, entsprechend dem Umfang dieses Kommandos. In der Tabelle sind deswegen nur die wichtigsten Befehle aufgelistet, um einen schnellen Einstieg zu ermöglichen. Tiefergehende Information sind über man rpm abrufbar. Eine sehr ausführliche Beschreibung der Möglichkeiten von rpm findet sich unter en http://www.rpm.org/max-rpm/.

Kompilieren von Source-RPMs
"""""""""""""""""""""""""""

Hat man ein Paket nur in Source-Form vorliegen (xxx.src.rpm), ist die Option --rebuild ganz hilfreich. Sie sorgt dafür, dass das Paket nach dem Auspacken auch gleich kompiliert wird. Während hierfür bei RPM-Versionen bis 4.0.X auch der Befehl rpm zuständig ist, gibt es seit der Version 4.1 den Befehl rpmbuild.

Das Kompilieren eines Source-RPMs auf dem eigenen Rechner hat auch den Vorteil, dass die Programme auf jeden Fall zu den installierten Bibliotheken passen.

Generell ist es empfehlenswert, diesen Kompilationsvorgang nicht als Benutzer  root durchzuführen. Um als normaler Benutzer einen rebuild durchzuführen, muß als erstes eine Datei .rpmmacros im Homeverzeichnis angelegt werden:
user@linux ~$ cat ~/.rpmmacros
%_topdir /tmp/mirko-redhat
user@linux ~$

Nun müssen noch einige Verzeichnisse angelegt werden:
user@linux ~$ mkdir /tmp/mirko-redhat
user@linux ~$ mkdir /tmp/mirko-redhat/SPECS
user@linux ~$ mkdir /tmp/mirko-redhat/BUILD
user@linux ~$ mkdir /tmp/mirko-redhat/SOURCES
user@linux ~$ mkdir /tmp/mirko-redhat/RPMS
user@linux ~$ mkdir /tmp/mirko-redhat/RPMS/i386
user@linux ~$ mkdir /tmp/mirko-redhat/RPMS/i686
user@linux ~$ mkdir /tmp/mirko-redhat/RPMS/noarch
user@linux ~$ mkdir /tmp/mirko-redhat/SRPMS

oder in einem Einzeiler:
user@linux ~$ mkdir -p /tmp/mirko-redhat/{RPMS/i386,RPMS/noarch,BUILD,SOURCES,SPECS,SRPMS}

Jetzt kann man ein vorhandenes Source-RPM einfach wie folgt kompilieren:
user@linux ~$ rpm --rebuild mod_auth_pam-1.0a-1.src.rpm

oder aber bei RPM-Versionen ab 4.1:
user@linux ~$ rpmbuild --rebuild mod_auth_pam-1.0a-1.src.rpm

Nach Ausführen des Befehls wird der Kompilationsvorgang durchgeführt:

    Die unter SOURCES abgelegten Quellen werden unterhalb von BUILD ausgepackt.
    Eventuell vorhandene Patches (Quelltext-Änderungen, die der Fehlerkorrektur oder dem Anpassen an das System dienen) verändern den Quelltext.
    Dann wird meistens automatisch der unter  Die klassische Installation beschriebene Ablauf aus ./configure, make, make install ausgeführt. Allerdings werden die Dateien hierbei temporär unter /var/tmp/PAKET-root installiert, da man als normaler Benutzer ja keine Zugriffsrechte auf die Standardverzeichnisse /usr, /etc usw. hat.
    Nun werden noch automatisch eventuell auftretende Abhängigkeiten aufgelöst.
    Die dem Programm zugehörigen Dateien werden komprimiert und in einem RPM zusammengefasst.

Am Ende findet sich dann unter RPMS/i386 das fertige RPM-Paket, welches man dann als  root installieren kann.

Anfragen der RPM-Datenbank
""""""""""""""""""""""""""

Neben den eigentlichen Programm- oder Source-Dateien, die gepackt vorliegen, enthalten RPM-Dateien zusätzliche Informationen, welche bei der Installation in einer Datenbank gespeichert werden. So umfasst ein RPM zusätzlich eine kurze Beschreibung des Programmes, den Installationszeitpunkt, die Zeit zu dem es kompiliert wurde, eine Auflistung aller dem Programm zugehörigen Dateien nebst Informationen über die Größe dieser Dateien und einen MD5-Hash, durch den sich nachträglich überprüfen lässt, ob die Dateien geändert wurden.

Auch sind in einem RPM die Abhängigkeiten von anderen Bibliotheken abgespeichert, so dass das Aufspielen einer neuen, inkompatiblen Bibliotheksversion durch den RedHat Package Manager verhindert wird. Außerdem lassen sich in einer RPM-Datei Skripte unterbringen, die vor bzw. nach der Installation bzw. Deinstallation eines Programmes automatisch ausgeführt werden. Diese können dann z.B. einen Dienst automatisch als zu startendes Programm eintragen oder einen neuen Benutzer hinzufügen (bei Datenbanken, Web- und Mailservern gebräuchlich) bzw. diese Aktionen bei der Deinstallation rückgängig machen.

Die in der Datenbank während der Installation eingetragenen Informationen lassen sich jederzeit abfragen (s. Tabelle)
Option/Argument 	Bedeutung 	Beispiel
-q query = Abfrage, 	ob ein Paket installiert ist 	rpm -q fileutils
-qa 	Anzeige aller installierten Pakete
-qf Dateiname 	zu welchem Paket gehört die Datei? 	rpm -qf /bin/ls => fileutils-4.1-4
-ql Paketname 	listet alle zum Paket gehörenden Dateien 	rpm -ql fileutils oder rpm -qlf /bin/ls
-qi Paketname 	Infos zur Version, Inhaltsangabe, Installationsdatum, etc. 	rpm -qi fileutils
-qd Paketname 	zeigt nur die zum Paket gehörenden Dokumentationsdateien an 	rpm -qd xinetd
-qc Paketname 	zum Paket gehörende Konfigurationsdateien 	rpm -qc xinetd
-q --changelog Paketname 	Anzeigen des RPM-ChangeLog, dieses muss nicht gleichbedeutend mit dem der Software sein, da die Distributoren die Sourcen oft noch patchen. 	rpm -q --changelog openssl

Viele dieser Abfrageoptionen lassen sich auch auf noch nicht installierte RPM-Pakete anwenden, hierzu dient die Option -p:
user@linux ~$ rpm -qip /mnt/cdrom/RedHat/RPMS/pinfo-0.5-1.i386.rpm

Graphische RPM-Frontends
""""""""""""""""""""""""

.. image:: iamges/software_installation_rpm-frontends.png

gnorpm, kpackage und xrpm

Wer mit der Kommandozeile des rpm-Kommandos auf Kriegsfuß steht oder Probleme hat, sich die wichtigsten Optionen zu behalten, hat die Auswahl zwischen mehreren graphischen Frontends, die aber nicht alle Optionen von rpm abdecken.

kpackage ist bei KDE dabei und unterstützt Drag & Drop, d. h. man kann ein heruntergeladenes Paket aus dem Datei-Manager heraus in kpackage hineinschieben und fallen lassen. Es versteht auch das de Debian-Paketformat, das an der Endung .deb erkennbar ist.

GnoRPM ist für Freunde des Gnome-Desktops.

xrpm ist ein in Python geschriebenes Frontend, das einfach zu bedienen ist und alle wichtigen Funktionen enthält.

mc -- der Midnight Commander ist zwar kein graphisches RPM-Frontend, kann aber RPM-Archive lesen und anzeigen 

Debian Paket Format
^^^^^^^^^^^^^^^^^^^

Das de Debian Paketformat ist detaillierter als RPM. Debian definiert nicht nur das Format, sondern auch die Datei-Struktur und vieles mehr. Deswegen ist das System problemloser aktualisierbar.

Während die meisten Distributionen inzwischen auf das RPM-Format umgestiegen sind, ist Debian seinem Paket-Format treu geblieben. Erkennbar sind diese Pakete an der Endung .deb. Zum Auspacken dient der Debian Packager (dpkg) oder das Kommando apt-get. dselect bietet ein Standard-Menü zur Paket-Installation, tasksel ein Menue mit verschiedenen vordefinierten Paketauswahlen. z.B. x-window-system oder mail-server.

Es gibt neben der Menü-gesteuerten Alternative (dselect, aptitude) auch graphische Frontends (gnome-apt, kpackage).
Kommando 	Beschreibung
apt-get install <paketname> 	Paket installieren
apt-get install <kernel-name> 	anderen Kernel installieren
apt-get --purge remove <paketname> 	Paket löschen
apt-get remove <paketname> 	Paket löschen, aber
Konfigurations-Dateien behalten
apt-get update / upgrade 	System auf den neuesten Stand bringen

Ausführliche Informationen zu apt erhalten Sie in dem Text  APT-Howto.

Da die Unterstützung von Debian-Paketen manchmal hinter der von RPM-Paketen hinterherhinkt, gibt es einen Konverter (alien), mit dem sich diese Pakete ins Debian-Format umwandeln lassen (und umgekehrt). Kritisch für eine Konvertierung sind systemnahen Paketen, da hier hierbei evtl. wichtige Informationen verloren gehen können.

Weitere Angaben zu Debian können dem Online-Manuel (man ...) oder dem Debian GNU/Linux Anwenderhandbuch (de http://www.openoffice.de/linux/buch/) entnommen werden.

Die klassische Installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Bevor Linux auf der Bildfläche erschien, wurden Programm-Pakete in Source-Form zur Verfügung gestellt, die in komprimierte Tar-Archive (auch als Tar-Ball bezeichnet) verpackt wurden. Während früher hauptsächlich das Unix-eigene compress zum Komprimieren verwendet wurde, ist es inzwischen weitgehend von gzip verdrängt worden, das einen besseren Komprimierungs-Faktor erzielt. Vereinzelt wird auch bzip2 eingesetzt (z. B. von http://www.blackdown.org), da es noch einen Tick besser ist (vgl. Abbildung "tar-archive.png"} -- hier wurde zum Vergleich die Tar-Datei von tkcvs 6.4 herangezogen)
Typische Komprimierung von compress, gzip und bzip2
Typische Komprimierung von compress, gzip und bzip2
Endung 	komprimiert mit 	auspacken mit
.tar 	(ohne) 	tar xvf ...
.tar.Z 	compress 	tar Zxvf ...
.tar.gz 	gzip 	tar zxvf ...
.tgz 	gzip 	tar zxvf ...
.tar.bz2 	bzip2 	tar jxvf ...

Das GNU-tar-Kommando, das üblicherweise bei allen Linux-Distributionen verwendet wird, kann mit komprimierten Tar-Archiven umgehen (s. Tabelle). Andere Unix-Systeme (z. B. SunOS) verwenden eine andere Tar-Implementierung. Hier muss man zuerst das Archiv dekomprimieren (mit uncompress, gunzip oder bunzip2), ehe man die Tar-Datei auspacken kann.

Vereinzelt findet man auch im Linux-Bereich Zip-Archive vor, erkennbar an der Endung .zip. Diese werden mit unzip ausgepackt.

Nachdem das Tar-Archiv erfolgreich ausgepackt ist, sollte man nach einer Datei README oder INSTALL Ausschau halten. Dort steht beschrieben, wie das Paket übersetzt und installiert wird. Unabhängig von der Plattform und Distribution sind es meist folgende Schritte, die ausgeführt werden:

    ./configure oder make config
    Im ersten Schritt wird untersucht, um was für ein System (Linux, Unix, ...) es sich handelt, welche Bibliotheken vorhanden sind und ob die zur Kompilierung benötigten Tools wie C-Compiler (gcc) oder Linker (ld) installiert sind, um daraus ein  Makefile zu generieren.
    make
    Mit Hilfe des  Makefiles, das im ersten Schritt erzeugt wurde, wird das Paket übersetzt.
    make test (optional)
    Mit diesem Schritt wird überprüft, ob die Kompilation erfolgreich war.
    make install
    Damit wird das Paket installiert.

Hilfreich bei der Übersetzung ist die Option -n des  make-Kommandos. Damit kann man make erst einmal trocken ausführen, um zu sehen, welche Kommandos alle ausgeführt werden und in welches Verzeichnis welche Dateien kopiert werden, um nötigenfalls das Makefile noch anpassen zu können.

Auch wenn dieses Verfahren meist problemlos funktioniert, hat die Sache einen Haken: an die Deinstallation hat der Autor meistens nicht gedacht, d. h. ein make uninstall wird in den wenigsten Fällen klappen. Und so bleiben die installierten Dateien bis in alle Ewigkeit im System, es sei denn, man hat sich bei der Installation gemerkt, welche Dateien wohin kopiert wurden und löscht sie manuell.

Weitere Nachteile der manuellen Installation:

    Auf dem Zielsystem müssen alle Werkzeuge (Compiler, Linker, Make etc.), Bibliotheken und Headerdateien zum Kompilieren des Programmes vorhanden sein.
    Bei der Installation einer neueren Version eines Programmes (Update) werden evtl. die bereits vorhandenen, an das System angepassten Konfigurationsdateien der alten Version überschrieben.

Perl-Archive
^^^^^^^^^^^^

Für Perl-Module gibt es als zentrale Anlaufstelle den CPAN-Server (Comprehensive Perl Archive Network, en http://www.cpan.org), über den fast alle Perl-Module bezogen und direkt installiert werden können.
user@linux ~$ perl -MCPAN -e 'install Data::JavaScript'

Mit diesem Aufruf wird das Data::JavaScript-Modul installiert. Beim ersten Mal muss man evtl. noch die automatische Installation konfigurieren. Dazu wird man interaktiv durch verschiedene Fragen durchgelotst (z. B. wo das \cmd{gzip}- und \cmd{tar}-Kommando liegt, \dots).

Danach geht es mit der eigentlichen Installation los, bei der das angegebene Modul von einem CPAN-Server heruntergeladen, ausgepackt, getestet und installiert wird. War alles erfolgreich, sollte am Ende ein
/usr/bin/make install  -- OK

zu sehen sein. Falls nicht, kann es evtl. daran liegen, dass das angegebene Modul noch von weiteren Modulen abhängt, die nicht auf dem System vorhanden sind. In diesem Fall sollte man zuerst diese Module noch installieren.

Selbstauspackende Archive
^^^^^^^^^^^^^^^^^^^^^^^^^

In seltenen Fällen kommen auch Shell-Skripte zum Einsatz, die sich nach dem Aufruf selbst auspacken. Eventuell muss man vorher noch einige Fragen zur Installation beantworten. Meistens heißt das Skript install.sh und wird mit
user@linux ~$ ./install.sh

oder
user@linux ~$ sh install.sh

aufgerufen. Lässt sich das Skript nicht ausführen, empfiehlt es sich, die erste Zeile zu überprüfen. Sie sollte ein
Ausgabe nach der Eingabe von install.sh

    
#!/bin/sh
    
    

enthalten, was leider nicht immer der Fall ist.

Software-Archive
^^^^^^^^^^^^^^^^

Linux ist Allgemeingut, dessen Bestandteile im Internet verstreut sind. Da es niemanden gehört, gibt es auch keine zentralen Stellen, die die ganzen Sourcen verwalten. Es gibt allerdings einige Anlaufstellen, von denen wir hier eine ganz kleine Auswahl präsentieren möchten (ohne Wertung):

    Sunsite
    Unter en http://sunsite.unc.edu/pub finden sich neben GNU-Projekten auch andere OpenSource-Projekte und über 55 GB an Linux-Software und -Dokumentationen.
    Rpmfind
    en http://rpmfind.net/linux/RPM ist ein riesiger Katalog von RPM-Archiven. Was man hier nicht findet, ist vermutlich auch nicht als RPM-Paket erhältlich.

Daneben gibt es natürlich noch die einzelnen Distributionen, die auch als Ausgangspunkt dienen können.

Autoren

    Oliver Boehm boehm@2xp.de
    Mirko Zeibig mirko-lists@zeibig.net
	
Formatierung

    Matthias Hagedorn matthias.hagedorn@selflinux.org


APT Howto
---------

Beschreibung

Dieses Dokument soll dem Benutzer ein gutes Verständnis für die Arbeitsweise des Debian-Paketmanagement-Werkzeuges APT liefern. Ziel ist es, das Leben für neue Debian-Benutzer zu erleichtern und denen zu helfen, die ihr Verständnis für die Administration dieses Systems vertiefen wollen.

Einführung
^^^^^^^^^^

Am Anfang war das .tar.gz. Benutzer mussten jedes Programm, welches Sie auf ihren GNU/Linux-Systemen benutzen wollten, selbst kompilieren. Zu Beginn der Entwicklung des Debian-Projekts erachtete man es für notwendig, dass das System eine Methode zum Verwalten der Pakete, die auf dem System installiert sind, enthält. Man gab dieser Methode den Namen dpkg. Dadurch war das erste Paket auf GNU/Linux geboren, bevor Red Hat sich entschied, ihr eigenes RPM-System zu erschaffen.

Schnell standen die Macher von GNU/Linux vor einem neuen Problem. Sie brauchten ein schnelles, praktisches und effizientes Mittel, um Pakete zu installieren, das Abhängigkeiten automatisch behandeln und ihre Konfigurationsdateien während des Aktualisierens berücksichtigen würde. Und wieder war es das Debian-Projekt, das den Weg machte und APT, das Advanced Packaging Tool, welches seitdem von Connectiva auf RPM portiert und von einigen anderen Distributionen übernommen wurde, das Licht der Welt erblicken ließ.

Diese Anleitung versucht nicht, apt-rpm (den Connectiva-Port von APT) zu behandeln.

Diese Dokumentation basiert auf der Debian-Version: Sarge. 

Basis-Konfiguration
^^^^^^^^^^^^^^^^^^^

Die Datei /etc/apt/sources.list
"""""""""""""""""""""""""""""""

Als Teil seiner Arbeit benutzt APT eine Datei, die die Quellen, von denen man Pakete beziehen kann, auflistet. Diese Datei heisst /etc/apt/sources.list.

Die Einträge in dieser Datei sind von folgendem Format:
/etc/apt/sources.list

deb http://site.http.org/debian distribution sektion1 sektion2 sektion3
deb-src http://site.http.org/debian distribution sektion1 sektion2 sektion3
     

Natürlich sind obige Einträge erfunden und sollten nicht benutzt werden. Das erste Wort jeder Zeile, deb oder deb-src zeigt den Typ des Archivs: Entweder es enthält Binär-Pakete (deb), das sind die vorkompilierten Pakete, die wir normalerweise benutzen, oder Quellpakete (deb-src), welche die originalen Programmquellen, die Debian-Kontrolldatei (.dsc) und das diff.gz, welches die Änderungen enthält, die für das Debianisieren des Programms von Nöten sind.

Normalerweise finden wir folgendes in der Standard-Debian-sources.list:
/etc/apt/sources.list

# See sources.list(5) for more information, especialy
# Remember that you can only use http, ftp or file URIs
# CDROMs are managed through the apt-cdrom tool.
deb http://http.us.debian.org/debian stable main contrib non-free
deb http://non-us.debian.org/debian-non-US stable/non-US main contrib non-free
deb http://security.debian.org stable/updates main contrib non-free

# Uncomment if you want the apt-get source function to work
#deb-src http://http.us.debian.org/debian stable main contrib non-free
#deb-src http://non-us.debian.org/debian-non-US stable/non-US main contrib non-free
     

Dieses sind die Zeilen, die eine Debian-Basis-Installation benötigt. Die erste deb-Zeile zeigt auf das offizielle Archiv, die zweite auf das Archiv non-US und die dritte auf das Archiv der Sicherheits-Updates von Debian.

Die letzten beiden Zeilen sind auskommentiert (mit einem # am Anfang). Deshalb wird apt-get sie ignorieren. Sie sind deb-src-Zeilen, das bedeutet, sie führen uns zu Debian-Quellpaketen. Wenn Sie öfters Programm-Quellen herunterladen, um sie zu testen oder neu zu kompilieren, sollten Sie die Kommentarzeichen entfernen.

Die Datei /etc/apt/sources.list kann verschiedene Typen von Zeilen enthalten. APT kann mit Archiven der Typen http, ftp und file (lokale Dateien, z. B. ein Verzeichnis, mit einem gemounteten ISO9660-Dateisystem).

Vergessen Sie nicht, apt-get update auszuführen, nachdem die /etc/apt-/sources.list editiert wurde. Dies ist notwendig, damit APT die Paketlisten der spezifizierten Quellen bezieht.

Wie man APT lokal benutzt
"""""""""""""""""""""""""

Manchmal haben Sie vielleicht einige .debs, bei denen Sie APT für die Installation benutzen wollen, so dass Abhängigkeiten automatisch aufgelöst werden.

Um das zu tun, erstellen Sie ein Verzeichnis und legen die .debs, die Sie indizieren wollen, dort hinein. Zum Beispiel:
root@linux # mkdir /root/debs/

Es ist möglich, die Definitionen der paketeigenen Kontrolldatei (debian/control) für das Repository mit Hilfe einer override-Datei zu übergehen. In dieser Datei können Sie einige Optionen definieren, die die paketeigenen Optionen überschreiben. Das Format sieht folgendermassen aus:

Paket Priorität Sektion
     

Paket ist der Name des Pakets, die Priorität ist low (niedrig), medium (mittel) oder high (hoch) und die Sektion ist die Sektion, zu der das Paket gehört. Der Dateiname spielt keine Rolle, er muss als Argument an dpkg-scanpackages übergeben werden. Wenn keine override-Datei gebraucht wird, kann man dpkg-scanpackages auch /dev/null übergeben.

Immer noch im Verzeichnis /root führen Sie folgendes aus:
root@linux # dpkg-scanpackages debs Datei | gzip > debs/Packages.gz

In der obenstehenden Zeile ist Datei die override-Datei. Das Kommando generiert eine Datei Packages.gz, welche verschiedene Informationen über die Pakete enthält, die APT benötigt. Um die Pakete benutzen zu können, fügen Sie folgendes der /etc/apt/sources.list hinzu:
/etc/apt/sources.list

deb file:/root debs/
     

Nachdem Sie das getan haben, können Sie einfach die gewöhnlichen APT-Kommandos benutzen. Sie können ebenfalls ein Quellarchiv erstellen. Die Prozedur ist dieselbe, aber die Dateien .orig.tar.gz, .dsc und .diff.gz müssen sich in dem Verzeichnis befinden und statt Packages.gz heisst es hier Sources.gz. Ausserdem müssen Sie ein anderes Programm benutzen. Es heisst dpkg-scansources. Das Kommando sieht folgendermassen aus:
root@linux # dpkg-scansources debs | gzip > debs/Sources.gz

dpkg-scansources braucht keine override-Datei. Die Zeile in der sources.list lautet:
/etc/apt/sources.list

deb-src file:/root debs/
     


Entscheidung - Welcher Mirror ist der beste für die sources.list: netselect, netselect-apt
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Eine häufige Frage, der meist neuen Benutzer ist: Welchen Debian-Mirror soll ich in die sources.list eintragen?. Es gibt viele Wege, sich für einen Mirror zu entscheiden. Die fortgeschritteneren Benutzer haben möglicherweise ein Skript, welches den Ping mehrerer Mirrors vergleicht. Aber es gibt so ein Programm inzwischen auch für weniger erfahrene Benutzer: netselect.

Installieren tut man netselect wie üblich:
root@linux # apt-get install netselect

Wenn man es ohne Parameter ausführt, zeigt es seinen Hilfetext an. Führt man es mit einer durch Leerzeichen separierten Liste von Hostnamen (Mirrors) aus, gibt es uns einen Hostnamen zusammen mit der einer Punktzahl zurück. Diese Punktzahl berücksichtigt die erwartete Pingzeit und die Zahl der Hops (Rechner, die eine Netzwerkanfrage passiert, um ihren Zielort zu erreichen) und ist antiproportional zur erwarteten Downloadgeschwindigkeit (also je niedriger, desto besser). Angezeigt wird nur der Host mit der niedrigsten Punktzahl (Die ganze Liste der Mirrors kann mit der Option -vv angesehen werden). Zum Beispiel:
root@linux # netselect ftp.debian.org http.us.debian.org ftp.at.debian.org download.unesp.br ftp.debian.org.br
365 ftp.debian.org.br

Das bedeutet, dass von den Mirrors, die als Parameter an netselect übergeben wurden, ftp.debian.org.br der beste war mit einer Punktzahl von 365. (Achtung! Weil es auf meinem Computer ausgeführt wurde und die Netzwerktopographie extrem unterschiedlich und abhängig vom Standort des Computers ist, ist dieser Wert nicht notwendigerweise die richtige Geschwindigkeit für andere Computer).

Jetzt tragen Sie einfach den schnellsten Mirror in die /etc/apt/sources.list ein (sehen Sie  Die Datei /etc/apt/sources.list) und befolgen Sie die Tips im Kapitel  Paketverwaltung.

Hinweis: Die Liste der Mirrors ist immer auf en http://www.debian.org/mirror/mirrors_full zu finden.

Ab Version 0.3 enthält das netselect-Paket das netselect-apt-Skript, das obigen Prozess automatisiert. Übergeben Sie einfach die Distribution als Parameter (Der Defaultwert ist stable) und die sources.list wird mit den besten main- und non-US-Mirrors generiert und im aktuellen Verzeichnis gespeichert. Das folgende Beispiel generiert eine sources.list für die stabile Distribution:
root@linux # ls sources.list
ls: sources.list: File or directory not found
root@linux # netselect-apt stable
(...)
root@linux # ls sources.list
sources.list

Hinweis: Die sources.list wird im aktuellen Verzeichnis erzeugt und muss nach /etc/apt verschoben werden.

Danach befolgen Sie die Tips im Kapitel  Paketverwaltung.

Hinzufügen einer CD-ROM in die sources.list
"""""""""""""""""""""""""""""""""""""""""""

Wenn Sie lieber eine CD-ROM zum Installieren von Paketen oder Updaten ihres Systems durch APT verwenden möchten, können Sie sie in Ihre sources.list eintragen. Um dieses zu tun, können Sie das Programm apt-cdrom, wie im folgenden beschrieben, benutzen:
root@linux # apt-cdrom add

Hierfür muss die Debian CD-ROM im Laufwerk liegen. Die CD-ROM wird gemountet, und wenn sie eine gültige Debian-CD ist, wird nach Paketinformationen gesucht. Wenn Ihre CD-ROM-Konfiguration ein wenig ungewöhnlich ist, können Sie die folgenden Optionen benutzen:
-h 	program help
-d directory 	CD-ROM mount point
-r 	Rename a recognized CD-ROM
-m 	No mounting
-f 	Fast mode, don't check package files
-a 	Thorough scan mode

Zum Beispiel:
root@linux # apt-cdrom -d /home/kov/mycdrom add

Eine CD kann auch identifiziert werden ohne sie zur sources.list hinzuzufügen:
root@linux # apt-cdrom ident

Obiges funktioniert nur, wenn das CD-ROM Laufwerk in der /etc/fstab korrekt konfiguriert ist.

Paketverwaltung
^^^^^^^^^^^^^^^

Update der Liste der verfügbaren Pakete
"""""""""""""""""""""""""""""""""""""""

Das Paketsystem benutzt eine eigene Datenbank mit Informationen über installierte, nicht installierte und für eine Installation verfügbare Pakete. Das Programm apt-get benutzt diese Datenbank, um herauszufinden, wie es die vom Benutzer angeforderten Pakete installieren soll und welche zusätzlichen Pakete benötigt werden, damit die ausgewählten Pakete ordentlich funktionieren.

Um diese Liste zu updaten, benutzt man das Kommando apt-get update. apt-get sucht dann nach den Paketlisten in den Archiven aus der /etc/apt/sources.list. Im Kapitel  Die Datei /etc/apt/sources.list finden Sie weitere Information über diese Datei.

Es ist eine gute Idee, dieses Kommando regelmäßig auszuführen, um sich selbst und sein System auf dem neusten Stand über mögliche Paket- bzw. Sicherheitsupdates zu halten.

Installieren von Paketen
""""""""""""""""""""""""

Endlich kommt das, worauf Sie alle gewartet haben! Mit der fertigen sources.list und der Liste der verfügbaren Pakete auf dem neusten Stand ist alles, was Sie zu tun haben apt-get auszuführen, um das gewünschte Paket zu installieren. Zum Beispiel:
root@linux # apt-get install xchat

APT durchsucht seine Datenbank nach der aktuellsten Version dieses Paketes und holt es aus dem entsprechenden Archiv, welches in der sources.list spezifiziert ist. Wenn es eintritt, dass das Paket von einem anderen abhängt -- wie es hier der Fall ist -- überprüft APT die Abhängigkeiten und installiert die benötigten Pakete. Sehen Sie folgendes Beispiel:
root@linux # apt-get install nautilus
Reading Package Lists... Done
Building Dependency Tree... Done
  The following extra packages will be installed:
  bonobo libmedusa0 libnautilus0
The following NEW packages will be installed:
  bonobo libmedusa0 libnautilus0 nautilus
0 packages upgraded, 4 newly installed, 0 to remove and 1  not upgraded.
Need to get 8329kB of archives. After unpacking 17.2MB will be used.
Do you want to continue? [Y/n]

Das Paket nautilus benötigt die genannten  Bibliotheken (bonobo libmedusa0 libnautilus0), deshalb holt APT sie aus dem Archiv. Übergibt man apt-get die Namen der  Bibliotheken beim Aufruf mit, fragt es nicht, ob es fortfahren soll, es akzeptiert automatisch, dass die genannten Pakete installiert werden sollen.

Das bedeutet, dass APT nur um Bestätigung bittet, wenn es Pakete installieren muss, die man nicht auf der Kommandozeile übergeben hat.

Die folgenden Optionen von apt-get können hilfreich sein:
-h 	Dieser Hilfetext
-d 	Nur herunterladen - Nicht installieren oder entpacken
-f 	Versuche fortzufahren wenn der integrity check fehlschlägt
-s 	Nichts wirklich tun. Simulation durchführen.
-y 	Beantworte alle Fragen mit Ja anstatt sie zu stellen.
-u 	Zeige eine Liste der Pakete die geupgraded werden.

Es können mehrere Pakete in einer Zeile zur Installation ausgewählt werden. Pakete, die über das Netzwerk oder Internet heruntergeladen wurden, werden im Verzeichnis /var/cache/apt/archives für spätere Installationen gespeichert.

Ebenfalls kann man Pakete zum Entfernen auf derselben Zeile angeben, indem man ein -direkt hinter den Paketnamen hängt wie im folgenden:
root@linux # apt-get install nautilus gnome-panel-
Reading Package Lists... Done
Building Dependency Tree... Done
The following extra packages will be installed:
  bonobo libmedusa0 libnautilus0
The following packages will be REMOVED:
  gnome-applets gnome-panel gnome-panel-data gnome-session
The following NEW packages will be installed:
  bonobo libmedusa0 libnautilus0 nautilus
0 packages upgraded, 4 newly installed, 4 to remove and 1  not upgraded.
Need to get 8329kB of archives. After unpacking 2594kB will be used.
Do you want to continue? [Y/n]

Im Abschnitt  Pakete entfernen finden Sie weitere Details zum Entfernen von Paketen.

Wenn Sie ein installiertes Paket irgendwie beschädigt haben oder einfach die Dateien eines Paketes mit der aktuellsten verfügbaren Version neu installieren möchten, können Sie die Option --reinstall wie im folgenden nutzen:
root@linux # apt-get --reinstall install gdm
Reading Package Lists... Done
Building Dependency Tree... Done
0 packages upgraded, 0 newly installed, 1 reinstalled, 0 to remove and
1 not upgraded.
Need to get 0B/182kB of archives. After unpacking 0B will be used.
Do you want to continue? [Y/n]

Die Version des APT, die zur Erstellung dieser Anleitung benutzt wurde, ist Version 0.5.3, die aktuelle Version in Debian unstable (sid) zur Zeit als sie geschrieben wurde. Wenn diese Version installiert ist, kann APT auf Ihren Wunsch noch mehr: Sie können ein Kommando der Form apt-get install paket/distribution benutzen, um ein Paket einer anderen Distribution zu installieren, oder apt-get install package=version. Zum Beispiel:
root@linux # apt-get install nautilus/unstable

Dies installiert nautilus aus der Distribution unstable, auch wenn die aktuell laufende Distribution stable ist. Mögliche Werte für distribution sind stable, testing, und unstable.

Meistens ist es besser, die Option -t zu benutzen, um eine Distribution zu wählen, was dazu führt, dass apt-get diese Distribution beim Auflösen von Abhängigkeiten bevorzugt.

WICHTIG: Die unstable-Version von Debian ist die Version, in welcher neue Versionen von Debian-Paketen zuerst erscheinen. Diese Distribution sieht alle Änderungen, die an Paketen vorgenommen werden, kleinere und größere, welche mehrere Pakete oder das ganze System betreffen können. Aus diesem Grund sollte sie nicht von unerfahrenen Benutzern oder solchen, die geprüfte Stabilität brauchen, verwendet werden.

Die testing-Distribution ist ein wenig besser als unstable was Stabilität angeht, jedoch sollte für Produktionssysteme die Distribution stable benutzt werden.

Pakete entfernen
""""""""""""""""

Wenn ein Paket nicht mehr gebraucht wird, kann es mit APT vom System entfernt werden. Geben Sie einfach apt-get remove package ein. Zum Beispiel:
root@linux # apt-get remove gnome-panel
Reading Package Lists... Done
Building Dependency Tree... Done
The following packages will be REMOVED:
  gnome-applets gnome-panel gnome-panel-data gnome-session
0 packages upgraded, 0 newly installed, 4 to remove and 1  not upgraded.
Need to get 0B of archives. After unpacking 14.6MB will be freed.
Do you want to continue? [Y/n]

Wie im obigen Beispiel zu sehen ist, kümmert sich APT ebenfalls um das Entfernen der Pakete, die das Paket, das Sie entfernen wollen, benötigen. Es gibt keine Möglichkeit, Pakete mit APT zu entfernen, ohne gleichzeitig die Pakete zu entfernen, die von dem entfernten Paket abhängen.

Wenn man apt-get ausführt wie oben angegeben, werden die Pakete entfernt, aber die Konfigurationsdateien, falls es welche gibt, bleiben auf dem System. Für eine komplette Entfernung der Pakete, sehen Sie folgendes Beispiel:
root@linux # apt-get --purge remove gnome-panel
Reading Package Lists... Done
Building Dependency Tree... Done
The following packages will be REMOVED:
  gnome-applets* gnome-panel* gnome-panel-data* gnome-session*
0 packages upgraded, 0 newly installed, 4 to remove and 1  not upgraded.
Need to get 0B of archives. After unpacking 14.6MB will be freed.
Do you want to continue? [Y/n]

Der * hinter den Namen der Pakete, die vom zu entfernenden Paket abhängen, bedeutet, dass deren Konfigurationsdateien ebenso entfernt werden.

Genau wie bei der Methode install kann man auch bei remove ein Symbol benutzen, um die Wirkung für ein einzelnes Paket umzukehren. Hier fügt man einem Paket ein + zu und das Paket wird installiert, anstatt entfernt zu werden.
root@linux # apt-get --purge remove gnome-panel nautilus+
Reading Package Lists... Done
Building Dependency Tree... Done
The following extra packages will be installed:
  bonobo libmedusa0 libnautilus0 nautilus
The following packages will be REMOVED:
  gnome-applets* gnome-panel* gnome-panel-data* gnome-session*
The following NEW packages will be installed:
  bonobo libmedusa0 libnautilus0 nautilus
0 packages upgraded, 4 newly installed, 4 to remove and 1  not upgraded.
Need to get 8329kB of archives. After unpacking 2594kB will be used.
Do you want to continue? [Y/n]

apt-get listet die Pakete auf, die extra installiert werden (die gebraucht werden, damit das Programm einwandfrei funktionieren kann), die entfernt werden und die installiert werden (hier werden die extra-Pakete noch einmal mit aufgelistet).

Upgrade von Paketen
"""""""""""""""""""

Das Aktualisieren von Paketen ist eine tolle Sache mit APT. Es braucht dafür nur einen einzigen Befehl: apt-get upgrade. Man kann diesen benutzen, um Pakete aus der gleichen Distribution zu aktualisieren, oder aus einer neuen Distribution, obwohl für letzteres apt-get dist-upgrade empfehlenswerter ist; siehe  Upgrade einer Debian-Version für weitere Einzelheiten.

Es ist sinnvoll, diesen Befehl mit der Option -u auszuführen. Diese Option lässt APT die komplette Liste der Pakete anzeigen, die aktualisiert werden sollen. Ohne diese Option aktualisiert man quasi blind. APT lädt die aktuellsten Versionen aller Pakete herunter und installiert sie in der richtigen Reihenfolge. Es ist wichtig, dass vor jedem Aktualisieren der Pakete apt-get update ausgeführt wird. Siehe Abschnitt  Update der Liste der verfügbaren Pakete. Zum Beispiel:
root@linux # apt-get -u upgrade
Reading Package Lists... Done
Building Dependency Tree... Done
The following packages have been kept back
  cpp gcc lilo
The following packages will be upgraded
  adduser ae apt autoconf debhelper dpkg-dev esound esound-common ftp
indent
  ipchains isapnptools libaudiofile-dev libaudiofile0 libesd0
libesd0-dev
  libgtk1.2 libgtk1.2-dev liblockfile1 libnewt0 liborbit-dev liborbit0
  libstdc++2.10-glibc2.2 libtiff3g libtiff3g-dev modconf orbit procps psmisc
29 packages upgraded, 0 newly installed, 0 to remove and 3 not upgraded.
Need to get 5055B/5055kB of archives. After unpacking 1161kB will be used.
Do you want to continue? [Y/n]

Das Ganze ist extrem einfach. Die ersten paar Zeilen sagen, dass einige Pakete zurückgehalten werden (have been kept back). Das bedeutet, dass es neuere Versionen dieser Pakete gibt, die aus irgendeinem Grund nicht installiert werden. Mögliche Gründe sind unerfüllbare Abhängigkeiten (z.B. wenn ein Paket, von dem das neue Paket abhängt, nicht im Archiv verfügbar ist) oder neue Abhängigkeiten (das Paket hängt nun von neuen Paketen ab).

Es gibt keine saubere Lösung für das erste Problem. Für den zweiten Fall kann man apt-get install für das spezielle Paket ausführen, das zurückgehalten wurde, da dann auch die Abhängigkeiten aufgelöst werden. Eine noch sauberere Lösung ist es, dist-upgrade zu benutzen. Siehe Abschnitt  Upgrade einer Debian-Version.

Upgrade einer Debian-Version
""""""""""""""""""""""""""""

Diese Funktion erlaubt es, ein ganzes Debian-System entweder über das Internet oder von einer neuen CD (die sie kaufen oder aus dem Internet herunterladen können) auf einmal zu aktualisieren.

Ausserdem ist es sinnvoll, wenn an den Abhängigkeiten zwischen den Paketen Änderungen vorgenommen wurden. Mit apt-get upgrade werden solche Pakete nicht installiert (sie werden auf dem derzeitigen Stand gehalten kept back).

Wenn auf Ihrem System z.B. Revision 0 der stabilen Debian-Version läuft und Sie sich Revision 3 auf CD kaufen, können Sie APT benutzen, um ein Upgrade auf die neue Version von CD durchzuführen. Dafür benutzen Sie apt-cdrom (siehe Abschnitt  Hinzufügen einer CD-ROM in die sources.list), um die CD zu ihrer /etc/apt/sources.list hinzuzufügen und führen Sie apt-get dist-upgrade aus.

Es ist wichtig zu wissen, dass APT immer nach der aktuellsten Version eines Pakets sucht. Wenn also Ihre /etc/apt/sources.list auf ein Archiv zeigt, das eine neuere Version eines Pakets enthält als sich auf der CD befindet, lädt APT das Paket aus diesem herunter.

In dem Beispiel aus  Upgrade von Paketen sehen wir, dass manche Pakete nicht aktualisiert wurden (kept back). Wir werden dieses Problem nun mit der Funktion dist-upgrade lösen:
root@linux # apt-get -u dist-upgrade
Reading Package Lists... Done
Building Dependency Tree... Done
Calculating Upgrade... Done
The following NEW packages will be installed:
  cpp-2.95 cron exim gcc-2.95 libident libopenldap-runtime libopenldap1
  libpcre2 logrotate mailx
The following packages have been kept back
  lilo
The following packages will be upgraded
  adduser ae apt autoconf cpp debhelper dpkg-dev esound esound-common
ftp gcc
  indent ipchains isapnptools libaudiofile-dev libaudiofile0 libesd0
  libesd0-dev libgtk1.2 libgtk1.2-dev liblockfile1 libnewt0 liborbit-dev
  liborbit0 libstdc++2.10-glibc2.2 libtiff3g libtiff3g-dev modconf orbit
  procps psmisc
31 packages upgraded, 10 newly installed, 0 to remove and 1 not upgraded.
Need to get 0B/7098kB of archives. After unpacking 3118kB will be used.
Do you want to continue? [Y/n]

Hier ist zu bemerken, dass die Pakete aktualisiert werden, aber neue Pakete (neue Abhängigkeiten der aktualisierten Pakete) zusätzlich installiert werden. Weiterhin wird lilo immer noch nicht aktualisiert, es gibt möglicherweise schwerwiegendere Probleme mit diesem Paket. Wir können das prüfen, in dem wir folgendes ausführen:
root@linux # apt-get -u install lilo
Reading Package Lists... Done
Building Dependency Tree... Done
The following extra packages will be installed:
  cron debconf exim libident libopenldap-runtime libopenldap1 libpcre2
  logrotate mailx
The following packages will be REMOVED:
  debconf-tiny
The following NEW packages will be installed:
  cron debconf exim libident libopenldap-runtime libopenldap1 libpcre2
  logrotate mailx
The following packages will be upgraded
  lilo
1 packages upgraded, 9 newly installed, 1 to remove and 31 not upgraded.
Need to get 225kB/1179kB of archives. After unpacking 2659kB will be used.
Do you want to continue? [Y/n]

Hier erfahren wir, dass das neue lilo einen Konflikt mit dem Paket debconf-tiny hat, was bedeutet, dass wir es nicht installieren (oder aktualisieren) können, ohne debconf-tiny zu entfernen.

Um herauszufinden, wovon ein Paket zurückgehalten oder entfernt wird, können Sie folgendes tun:
root@linux # apt-get -o Debug::pkgProblemResolver=yes dist-upgrade
Reading Package Lists... Done
Building Dependency Tree... Done
Calculating Upgrade... Starting
Starting 2
Investigating python1.5
Package python1.5 has broken dep on python1.5-base
  Considering python1.5-base 0 as a solution to python1.5 0
  Holding Back python1.5 rather than change python1.5-base
Investigating python1.5-dev
Package python1.5-dev has broken dep on python1.5
  Considering python1.5 0 as a solution to python1.5-dev 0
  Holding Back python1.5-dev rather than change python1.5
Try to Re-Instate python1.5-dev
Done
Done
The following packages have been kept back
  gs python1.5-dev
0 packages upgraded, 0 newly installed, 0 to remove and 2  not upgraded.

Auf diesem Wege ist es einfach festzustellen, dass das Packet python1.5-dev wegen einer ungelösten Abhängigkeit zu python1.5 nicht installiert werden kann.

Ungenutzte Pakete entfernen: apt-get clean and autoclean
""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Wenn ein Paket installiert werden soll, bezieht APT von den Quellen, die in der /etc/apt/sources.list aufgelistet sind, die nötigen Dateien, legt sie in ein lokales Archiv (/var/cache/apt/archives/) und fährt mit der Installation fort. (sehen Sie  Installieren von Paketen).

Nach und nach kann dieses lokale Archiv immer größer werden und eine Menge Platz auf der Festplatte belegen. Auch für diesen Fall bietet APT Werkzeuge an, um sein lokales Archiv zu warten: apt-get clean und autoclean Methoden.

apt-get clean entfernt alles bis auf Lock-Dateien aus /var/cache/apt/archives/ und /var/cache/apt/archives/partial/. In der Folge muss APT ein Paket, das Sie erneut installieren wollen auch erneut herunterladen.

apt-get autoclean entfernt nur Pakete, die nicht mehr heruntergeladen werden können.

Das folgende Beispiel sollte zeigen, wie apt-get autoclean arbeitet:
root@linux # ls /var/cache/apt/archives/logrotate* /var/cache/apt/archives/gpm*
logrotate_3.5.9-7_i386.deb
logrotate_3.5.9-8_i386.deb
gpm_1.19.6-11_i386.deb

In /var/cache/apt/archives liegen zwei Versionen des Pakets logrotate und eine Version des Pakets gpm.
root@linux # apt-show-versions -p logrotate
logrotate/stable uptodate 3.5.9-8
root@linux # apt-show-versions -p gpm
gpm/stable upgradeable from 1.19.6-11 to 1.19.6-12

apt-show-versions zeigt, dass logrotate_3.5.9-8_i386.deb die aktuelle Version von logrotate bereitstellt, daher wird logrotate_3.5.9-7_i386.deb nicht mehr benötigt. Ebenso wird gpm_1.19.6-11_i386.deb nicht mehr benötigt, da eine aktuellere Version von den Debian-Archiven heruntergeladen werden kann.
root@linux # apt-get autoclean
Reading Package Lists... Done
Building Dependency Tree... Done
Del gpm 1.19.6-11 [145kB]
Del logrotate 3.5.9-7 [26.5kB]

apt-get autoclean entfernt also nur die alten Pakete. Für weitere Informationen über apt-show-versions sehen Sie  Upgrade von Paketen spezieller Debian-Versionen.

APT unter dselect verwenden
"""""""""""""""""""""""""""

dselect ist ein Programm, das Debian-Benutzern hilft, zu installierende Pakete auszuwählen. Viele halten es für zu kompliziert und vielmehr langweilig, aber mit ein wenig Übung kann man durchaus Gefallen an seiner konsolen-basierten ncurses-Oberfläche finden.

Eine Stärke von dselect ist es, dass es mit den Möglichkeiten umgehen kann, die Debian-Pakete haben, um andere Pakete zu empfehlen (suggesting) oder vorzustellen (recommending). Um es zu benutzen, rufen Sie dselect als root auf. Wählen Sie apt als Zugriffsmethode. Das ist zwar nicht dringend notwendig, aber wenn Sie keine CD-ROM benutzen und Pakete aus dem Internet herunterladen möchten, ist es der beste Weg.

Um ein besseres Verständnis über die Benutzung von dselect zu erhalten, lesen sie die Dokumentation auf der Debian-Homepage de http://www.debian.org/doc/ddp.

Nachdem Sie Ihre Auswahl mit dselect getroffen haben, benutzen Sie folgendes Kommando
root@linux # apt-get -u dselect-upgrade

wie im folgenden Beispiel:
root@linux # apt-get -u dselect-upgrade
Reading Package Lists... Done
Building Dependency Tree... Done
The following packages will be REMOVED:
  lbxproxy
The following NEW packages will be installed:
  bonobo console-tools-libs cpp-3.0 enscript expat fingerd gcc-3.0
  gcc-3.0-base icepref klogd libdigest-md5-perl libfnlib0 libft-perl
  libgc5-dev libgcc300 libhtml-clean-perl libltdl0-dev libsasl-modules
  libstdc++3.0 metamail nethack proftpd-doc psfontmgr python-newt talk
tidy
  util-linux-locales vacation xbill xplanet-images
The following packages will be upgraded
  debian-policy
1 packages upgraded, 30 newly installed, 1 to remove and 0  not upgraded.
Need to get 7140kB of archives. After unpacking 16.3MB will be used.
Do you want to continue? [Y/n]

Im Vergleich: apt-get dist-upgrade auf demselben System:
root@linux # apt-get -u dist-upgrade
Reading Package Lists... Done
Building Dependency Tree... Done
Calculating Upgrade... Done
The following packages will be upgraded
  debian-policy
1 packages upgraded, 0 newly installed, 0 to remove and 0  not upgraded.
Need to get 421kB of archives. After unpacking 25.6kB will be freed.
Do you want to continue? [Y/n]

Viele der Pakete im oberen Beispiel werden installiert, weil andere Pakete sie empfehlen (suggest or recommend). Andere werden aufgrund unserer Auswahl, die wir beim Navigieren durch die Paketlisten von dselect getroffen haben, installiert oder entfernt (Im Falle von lbxproxy z. B.). dselect kann in Verbindung mit APT ein nützliches Werkzeug sein.

Wartung eines "gemischten" Systems
""""""""""""""""""""""""""""""""""

Viele benutzen die testing-Distribution, da sie stabiler ist als unstable und aktueller als stable ist. Benutzer, die aktuelle Versionen von Paketen wollen, sich aber nicht trauen, ihr ganzes System auf unstable umzustellen, haben die Möglichkeit testing und unstable zu mischen. Auf der anderen Seite möchten konservativere Benutzer vielleicht stable und testing mischen.

Für diesen Zweck muss die folgende Zeile in die /etc/apt/apt.conf eingefügt werden:
/etc/apt/apt.conf

APT::Default-Release "testing";
     

Um nun Pakete aus unstable zu installieren, muss die Option -t benutzt werden:
root@linux # apt-get -t unstable install Paketname

Vergessen Sie aber nicht, dass sie, um ein Paket aus einer anderen Debian Version zu installieren, eine Quellzeile in die /etc/apt/sources.list einfügen müssen. In unserem Beispiel brauchen wir Quellzeilen für die unstable-Distribution neben denen für testing.

Upgrade von Paketen spezieller Debian-Versionen
"""""""""""""""""""""""""""""""""""""""""""""""

apt-show-versions bietet einen sicheren Weg für Benutzer gemischter Systeme, um ihre Systeme zu aktualisieren, ohne mehr aus der weniger stabilen Distribution zu installieren als sie im Sinn haben. Zum Beispiel ist es möglich, nur die unstable-Pakete zu aktualisieren, in dem man folgendes ausführt:
root@linux # apt-get install `apt-show-versions -u -b | grep unstable`

Wie man bestimmte Versionen eines Paketes behält (komplex)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Manchmal gibt es Gründe, etwas in einem Paket zu verändern, und es fehlt die Zeit oder die Lust, diese Dinge auf neue Versionen des Paketes zu übertragen. Vielleicht haben Sie auch gerade ihre Debian-Version auf 3.0 aktualisiert, aber möchten trotzdem ein Paket aus Version 2.2 behalten. Es ist möglich, die installierte Version zu markieren (pin), so dass sie nicht aktualisiert wird.

Diese Möglichkeit einzusetzen ist einfach. Editieren Sie einfach die Datei /etc/apt/preferences.

Das Format ist trivial:
/etc/apt/preferences

Package: <Paket>
Pin: <Pin-Definition>
Pin-Priority: <Priorität des Pins>
     

Um zum Beispiel das Paket sylpheed in Version 0.4.99 zu behalten, fügen wir folgendes hinzu:
/etc/apt/preferences

Package: sylpheed
Pin: version 0.4.99*
     

Beachten Sie das * (Sternchen/Asterisk). Es funktioniert als Platzhalter; das bedeutet, dass dieser Pin für alle Versionen, die mit 0.4.99 beginnen, gültig sein soll. Das ist nötig, da Pakete in Debian eine Nummer für die Debian-Revision enthalten und ich nicht verhindern möchte, dass diese Revisionen installiert werden. Folglich werden die Versionen 0.4.99-1 und 0.4.99-10 installiert, sobald sie verfügbar sind. Beachten Sie, dass Sie das vermutlich nicht möchten, wenn Sie das Paket modifiziert haben, da diese Änderungen dann verloren gehen.

Das Feld Pin-Priority ist optional; wenn nicht anders spezifiziert, hat der Pin die Priorität 989.

Lassen Sie uns einen Blick darauf werfen, wie die Pin-Priorität funktioniert. Eine niedrigere Priorität als Null bewirkt, dass das Paket nie installiert wird. Prioritäten von 0 bis 100 bezeichnen Pakete, die nicht installiert sind und keine verfügbare Version haben. Solche Pakete werden in der Auswahl verfügbarer Versionen nicht berücksichtigt. Die Priorität 100 bezeichnet ein installiertes Paket. Damit eine installierte Version von einer anderen ersetzt wird, muss die Priorität über 100 liegen.

Prioritäten über 100 sagen aus, dass ein Paket installiert werden soll. Normalerweise wird ein installiertes Paket nur durch neuere Versionen ersetzt. Alle Prioritäten zwischen 100 und 1000 (inklusive) führen zu diesem Normalverhalten. Ein Paket mit solcher Priorität wird nicht durch eine niedrigere Version ersetzt. Wenn ich also zum Beispiel sylpheed 0.5.3 installiert habe und einen Pin auf sylpheed 0.4.99 mit der Priorität 999 definiert habe, wird Version 0.4.99 nicht installiert werden, um den Pin zu erfüllen. Um Pakete deaktualisieren zu können, um einen Pin zu erfüllen, braucht der Pin eine Priorität von über 1000.

Ein Pin kann für die Version, das Release oder die Herkunft (origin) definiert werden.

Für einen Pin auf die Version, wie oben beschrieben, kann man sowohl Versionsnummern als auch Platzhalter (Sternchen) verwenden. Letzteres spezifiziert mehrere Versionen in einem Pin.

Die Option Release benutzt die Release-Datei aus den APT-Archiven oder von den CDs. Die Brauchbarkeit dieser Version verfällt, wenn Sie APT-Archive benutzen, die diese Datei nicht zur Verfügung stellen. Sie können den Inhalt der Release-Dateien, die Sie haben in /var/lib/apt/lists/ nachlesen. Die Parameter für ein Release sind: a (Archiv(Archive)), c (Sektion(Component)), v (Version(Version)), o (Herkunft(Origin)) und l (Label(Label)).

Beispiel:
/etc/apt/preferences

Package: *
Pin: release v=2.2*,a=stable,c=main,o=Debian,l=Debian
Pin-Priority: 1001
     

In diesem Beispiel wählen wir die Debian-Version 2.2* (was 2.2r2, 2.2r3 sein kann -- r* bezeichnet so genannte point releases, welche normalerweise Sicherheitsupdates und andere extrem wichtige Updates enthalten), das stable Archiv, die Sektion main (im Gegensatz zu contrib oder non-free) und Herkunft und Label Debian. Herkunft (o=) bezeichnet, wer die Release-Datei erstellt hat, das Label (l=) definiert den Namen der Debian-Distribution: Debian für Debian selbst und Progeny für Progeny Linux zum Beispiel. Ein Beispiel einer Release-Datei:
root@linux # cat /var/lib/apt/lists/ftp.debian.org.br_debian_dists_potato_main_binary-i386_Release
Archive: stable
Version: 2.2r3
Component: main
Origin: Debian
Label: Debian
Architecture: i386

Sehr nützliche Helfer
^^^^^^^^^^^^^^^^^^^^^

Installieren selbstkompilierter Pakete: equivs
""""""""""""""""""""""""""""""""""""""""""""""

Manchmal will man spezielle Versionen eines Programms benutzen, die nur als Quellcode verfügbar sind und nicht als Debian-Paket. Hier kann es allerdings Probleme mit dem Paket-System geben. Angenommen Sie wollen eine neue Version ihres Mailservers kompilieren und alles klappt, aber viele Pakete in Debian hängen von einem MTA (Mail Transfer Agent) ab. Da etwas installiert wurde, was Sie selbst kompiliert haben, weiss das Paketsystem darüber nicht Bescheid.

Hier kommt das equivs ins Spiel. Um es zu benutzen, installieren Sie das Paket mit diesem Namen. Es erstellt ein leeres Paket, das die Abhängigkeiten erfüllt und dem Paketsystem mitteilt, so dass es keine Probleme mit Abhängigkeiten gibt.

Bevor wir näher darauf eingehen, ist es wichtig, Sie darauf hinzuweisen, dass es sicherere Möglichkeiten gibt, Programme, für die in Debian schon Pakete existieren, mit anderen Optionen zu kompilieren und man equivs nicht benutzen sollte, um Abhängigkeiten zu entfernen, ohne genau zu wissen, was man tut. Siehe  Das Arbeiten mit Quellpaketen für nähere Informationen.

Lassen Sie uns mit dem MTA-Beispiel fortfahren. Sie haben also gerade ihren frisch kompilierten postfix installiert und wollen nun mutt (ein Mailprogramm) installieren. Plötzlich stellen Sie fest, dass  mutt einen anderen MTA installieren möchte, obwohl Sie schon ihren selbstkompilierten MTA laufen haben.

Wechseln Sie in irgendein Verzeichnis (z. B. /tmp), und führen Sie folgendes aus:
root@linux # equivs-control name

Ersetzen Sie name durch den Namen der Kontrolldatei, die Sie erstellen wollen. Die Datei wird wie folgt erstellt:

Section: misc
Priority: optional
Standards-Version: 3.0.1

Package: <Paketname; wenn nicht angegeben: equivs-dummy>
Version: <Versionsnummer; wenn nicht angegeben: 1.0>
Maintainer: <Ihr Name mit Emailadresse; wenn nicht angegeben: Benutzername>
Pre-Depends: <Pakete>
Depends: <Pakete>
Recommends: <Pakete>
Suggests: <Pakete>
Provides: <(virtuelles) Paket>
Architecture: all
Copyright: <copyright Datei; normalerweise GPL2>
Changelog: <changelog file; normalerweise ein generisches Changelog>
Readme: <README.Debian file; wenn nicht angegeben, ebenfalls ein generisches>
Extra-Files: <Zusätzliche Dateien für das doc-Verzeichnis, kommasepariert>
Description: <kurze Beschreibung; Standard ist "some wise words">
 Lange Beschreibung und Info
 .
 Zweiter Paragraph
     

Nun muss das so angepasst werden, dass es tut, was wir wollen. Schauen Sie sich die Felder und ihre Beschreibungen an, es ist nicht nötig, jedes einzelne hier zu erklären, lassen Sie uns das Nötigste tun:

Section: misc
Priority: optional
Standards-Version: 3.0.1

Package: mta-local
Provides: mail-transport-agent
     

Das war es schon.  mutt hängt von mail-transport-agent ab, was ein virtuelles Paket ist, das alle MTAs liefern. Ich hätte das Paket einfach mail-transport-agent nennen können, aber ich bevorzugte das Schema für virtuelle Pakete, welches das Feld Provides benutzt.

Nun muss das Paket nur noch gebaut werden:
root@linux # equivs-build name
dh_testdir
touch build-stamp
dh_testdir
dh_testroot
dh_clean -k
# Add here commands to install the package into debian/tmp.
touch install-stamp
dh_testdir
dh_testroot
dh_installdocs
dh_installchangelogs
dh_compress
dh_fixperms
dh_installdeb
dh_gencontrol
dh_md5sums
dh_builddeb
dpkg-deb: building package `name' in `../name_1.0_all.deb'.

The package has been created.
Attention, the package has been created in the current directory,

Und nun installieren Sie das erzeugte .deb.

Wie man unschwer erkennen kann, gibt es verschiedene Anwendungen für equivs. Man könnte sogar ein Favoriten-Paket erstellen, was von den Paketen abhängt, die Sie normalerweise installieren. Lassen Sie Ihren Vorstellungen einfach freien Lauf, aber seien Sie vorsichtig.

Es ist wichtig zu erwähnen, dass es in /usr/share/doc/equivs/examples einige Beispiel-Kontrolldateien gibt. Werfen Sie mal einen Blick darauf.

Entfernen von unbenutzten locale-Dateien: localepurge
"""""""""""""""""""""""""""""""""""""""""""""""""""""

Viele Debian-Benutzer verwenden nur ein locale (Spracheinstellung). Ein brasilianischer Debian-Benutzer benutzt z. B. vermutlich immer das brasilianische pt_BR-locale und interessiert sich nicht für das spanische es-locale.

localepurge ist ein sehr nützliches Werkzeug für diese Art von Benutzern. Sie können eine Menge Festplattenplatz sparen, wenn Sie nur die locales installiert haben, die Sie auch wirklich brauchen. Installieren Sie einfach apt-get install localepurge.

Es ist wirklich einfach zu konfigurieren, Debconf-Fragen führen den Benutzer Schritt für Schritt durch die Konfiguration. Seien Sie vorsichtig beim Beantworten der ersten Frage, da falsche Antworten alle locales entfernen können - selbst die, die Sie benutzen. Die einzige Möglichkeit, sie wiederherzustellen, ist, alle Pakete neu zu installieren, die sie enthalten.

Erfahren, welche Pakete aktualisiert werden können
""""""""""""""""""""""""""""""""""""""""""""""""""

apt-show-versions ist ein Programm, das zeigt, welche Pakete im System aktualisiert werden können und andere hilfreiche Informationen bietet. Die Option -u zeigt eine Liste der Pakete, die aktualisiert werden können:
root@linux # apt-show-versions -u
libeel0/unstable upgradeable from 1.0.2-5 to 1.0.2-7
libeel-data/unstable upgradeable from 1.0.2-5 to 1.0.2-7

Informationen über Pakete
^^^^^^^^^^^^^^^^^^^^^^^^^

Es gibt einige Oberflächen für das APT-System, die es signifikant einfacher machen, Listen über verfügbare Pakete oder schon installierte Pakete zu bekommen oder auch herauszufinden, zu welcher Sektion ein Paket gehört, welche Priorität es hat, wie seine Beschreibung lautet, etc.

Unser Ziel aber hier ist, APT selbst benutzen zu lernen. Wie können wir also den Namen eines Paketes herausfinden, welches wir installieren wollen?

Es gibt eine Reihe von Möglichkeiten für eine solche Aufgabe. Fangen wir mit apt-cache an. Dieses Programm wird vom APT-System zum Warten seiner Datenbank benutzt. Werfen wir nur einen kleinen Blick auf einige seiner praktischeren Anwendungen.

Paketnamen entdecken
""""""""""""""""""""

Angenommen Sie wollen die alten Zeiten des Atari 2600 wieder aufleben lassen. Sie möchten APT benutzen, um einen Atari-Emulator zu installieren und dann Spiele herunterladen. Sie haben folgende Möglichkeit:
root@linux # apt-cache search atari
atari-fdisk-cross - Partition editor for Atari (running on non-Atari)
circuslinux - The clowns are trying to pop balloons to score points!
madbomber - A Kaboom! clone
tcs - Character set translator.
atari800 - Atari emulator for svgalib/X/curses
stella - Atari 2600 Emulator for X windows
xmess-x - X binaries for Multi-Emulator Super System

Wir finden verschiedene Pakete mit kurzen Beschreibungen. Um weitere Informationen über ein bestimmtes Paket zu erhalten, können wir folgendes machen:
root@linux # apt-cache show stella
Package: stella
Priority: extra
Section: non-free/otherosfs
Installed-Size: 830
Maintainer: Tom Lear <tom@trap.mtview.ca.us>
Architecture: i386
Version: 1.1-2
Depends: libc6 (>= 2.1), libstdc++2.10, xlib6g (>= 3.3.5-1)
Filename: dists/potato/non-free/binary-i386/otherosfs/stella_1.1-2.deb
Size: 483430
MD5sum: 11b3e86a41a60fa1c4b334dd96c1d4b5
Description: Atari 2600 Emulator for X windows
Stella is a portable emulator of the old Atari 2600 video-game console
written in C++.  You can play most Atari 2600 games with it.  The latest
news, code and binaries for Stella can be found at:
http://www4.ncsu.edu/~bwmott/2600

Mit dieser Ausgabe erhalten Sie eine Menge Details über das Paket, das Sie installieren wollen (oder nicht wollen) inklusive der vollständigen Beschreibung des Pakets. Wenn das Paket schon installiert ist und es eine neuere Version gibt, bekommen Sie Informationen über beide Versionen. Beispiel:
root@linux # apt-cache show lilo
Package: lilo
Priority: important
Section: base
Installed-Size: 271
Maintainer: Russell Coker <russell@coker.com.au>
Architecture: i386
Version: 1:21.7-3
Depends: libc6 (>= 2.2.1-2), debconf (>=0.2.26), logrotate
Suggests: lilo-doc
Conflicts: manpages (>>1.29-3)
Filename: pool/main/l/lilo/lilo_21.7-3_i386.deb
Size: 143052
MD5sum: 63fe29b5317fe34ed8ec3ae955f8270e
Description: LInux LOader - The Classic OS loader can load Linux and
others
This Package contains lilo (the installer) and boot-record-images to
install Linux, OS/2, DOS and generic Boot Sectors of other OSes.
.
You can use Lilo to manage your Master Boot Record (with a simple text screen)
or call Lilo from other Boot-Loaders to jump-start the Linux kernel.

Package: lilo
Status: install ok installed
Priority: important
Section: base
Installed-Size: 190
Maintainer: Vincent Renardias <vincent@debian.org>
Version: 1:21.4.3-2
Depends: libc6 (>= 2.1.2)
Recommends: mbr
Suggests: lilo-doc
Description: LInux LOader - The Classic OS loader can load Linux and
others
This Package contains lilo (the installer) and boot-record-images to
install Linux, OS/2, DOS and generic Boot Sectors of other OSes.
.
You can use Lilo to manage your Master Boot Record (with a simple text screen)
or call Lilo from other Boot-Loaders to jump-start the Linux kernel.

Das erste in der Liste ist die neu verfügbare Version und das zweite die installierte Version. Für generellere Informationen über ein Paket können sie folgendes benutzen:
root@linux # apt-cache showpkg penguin-command
Package: penguin-command
Versions:
1.4.5-1(...)(/var/lib/dpkg/status)

Reverse Depends:
Dependencies:
1.4.5-1 - libc6 (2 2.2.1-2) libpng2 (0 (null)) libsdl-mixer1.1 (2 1.1.0) libsdl1.1 (0 (null)) zlib1g (2 1:1.1.3)
Provides:
1.4.5-1 -
Reverse Provides:

Und um nur herauszufinden, von welchen Paketen es abhängt:
root@linux # apt-cache depends penguin-command
penguin-command
  Depends: libc6
  Depends: libpng2
  Depends: libsdl-mixer1.1
  Depends: libsdl1.1
  Depends: zlib1g

Zusammengefasst haben wir eine handvoll Waffen, die wir benutzen können, um den Namen des Paketes herauszufinden, das wir installieren wollen.

Paketnamen mit dpkg finden
""""""""""""""""""""""""""

Ein Weg, den Namen eines Pakets zu finden, ist, den Namen einer wichtigen Datei zu kennen, die sich in dem Paket befindet. Um zum Beispiel das Paket zu finden, welches eine bestimmte .h Datei enthält, die für das Kompilieren eines Programms benötigt wird, ist folgendes auszuführen:
root@linux # dpkg -S stdio.h
libc6-dev: /usr/include/stdio.h
libc6-dev: /usr/include/bits/stdio.h
perl: /usr/lib/perl/5.6.0/CORE/nostdio.h

oder:
root@linux # dpkg -S /usr/include/stdio.h
libc6-dev: /usr/include/stdio.h

Um den Namen installierter Pakete herauszufinden, was zum Beispiel zum Aufräumen der Festplatte nützlich sein kann, benutzen Sie:
root@linux # dpkg -l | grep mozilla
ii  mozilla-browse 0.9.6-7        Mozilla Web Browser

Das Problem mit diesem Befehl ist, dass er Paketnamen brechen kann. Im obigen Beispiel ist der ganze Name des Pakets mozilla-browser. Um das Problem zu beheben, können Sie die Umgebungsvariable COLUMNS folgendermaßen benutzen:
root@linux # COLUMNS=132 dpkg -l | grep mozilla
ii  mozilla-browser             0.9.6-7                     Mozilla Web
Browser - core and browser

oder die Beschreibung bzw. einen Teil dieser wie im folgenden:
root@linux # apt-cache search "Mozilla Web Browser"
mozilla-browser - Mozilla Web Browser

Pakete nach Bedarf installieren
"""""""""""""""""""""""""""""""

Sie kompilieren gerade ein Programm, und es gibt einen Fehler, da eine .h Datei gebraucht wird, die Sie nicht haben. Das Programm auto-apt kann Sie vor solchen Szenarios bewahren. Es fragt, ob es die benötigten Pakete installieren soll, nachdem es den betroffenden Prozess gestoppt hat und führt ihn fort, wenn die relevanten Pakete installiert sind.

Der Befehl sieht folgendermassen aus:
root@linux # auto-apt run Kommando

Wobei Kommando das Kommando ist, das ausgeführt werden soll und evtl. nicht vorhandene Dateien benötigt. Beispiel:
root@linux # auto-apt run ./configure

Es wird fragen, ob die benötigten Pakete installiert werden sollen, und apt-get automatisch aufrufen. Wenn  X läuft, ersetzt eine grafische Oberfläche die übliche Text-Oberfläche.

Auto-apt funktioniert mit einer Datenbank welche aktuell gehalten werden muss, um effektiv zu funktionieren. Das erreicht man mit den Kommandos auto-apt update, auto-apt updatedb und auto-apt update-local.

Herausfinden, zu welchem Paket eine Datei gehört
""""""""""""""""""""""""""""""""""""""""""""""""

Wenn ein Paket installiert werden soll und Sie nicht herausfinden können, wie es heißt, indem Sie mit apt-cache suchen, aber den Dateinamen des Programms oder einer Datei, die zu dem Paket gehört kennen, können Sie apt-file benutzen, um den Dateinamen zu finden. Das wird folgendermaßen gemacht:
root@linux # apt-file search Dateinamen

Es funktioniert genau wie dpkg -S, es zeigt Ihnen aber auch nicht installierte Pakete, die die Datei enthalten. Man kann es auch dazu benutzen, benötigte include-Dateien, die beim Kompilieren von Programmen fehlen, zu installieren, allerdings ist auto-apt eine wesentlich bessere Methode solche Fälle zu lösen, siehe  Pakete nach Bedarf installieren.

Man kann auch den Inhalt von Paketen auflisten:
root@linux # apt-file list Paketname

apt-file hat genau wie auto-apt eine Datenbank über die Dateien aller Pakete und diese muss aktuell gehalten werden:
root@linux # apt-file update

Normalerweise benutzt apt-file die gleiche Datenbank wie auto-apt, sehen Sie  Pakete nach Bedarf installieren.

Über Änderungen in Paketen informiert bleiben
"""""""""""""""""""""""""""""""""""""""""""""

Jedes Paket installiert in sein Dokumentationsverzeichnis (/usr/share/doc/Paketname) eine Datei mit Namen changelog.Debian.gz, welche die Liste der Änderungen gegenüber der letzten Version enthält. Sie können diese Dateien z. B. mit Hilfe von zless lesen, aber es ist nicht wirklich leicht, nach einem System-Upgrade nach dem changelog jedes aktualisierten Paketes zu suchen.

Es gibt aber eine Möglichkeit, diese Aufgabe zu automatisieren mit Hilfe eines Werkzeugs mit Namen apt-listchanges. Hierfür muss das Paket apt-listchanges erst einmal installiert werden. Während der Installation übernimmt Debconf die Installation. Beantworten Sie die Fragen nach Ihren Bedürfnissen.

Die Option Soll apt-listchanges nach dem Anzeigen der Changelogs um eine Bestätigung bitten? ist sehr nützlich, da es eine Liste der Änderungen jedes Paketes, das während eines Upgrades installiert wird, anzeigt und Ihnen die Möglichkeit bietet, diese vor dem Fortfahren einzusehen. Wenn Sie hier sagen, dass Sie nicht fortfahren möchten, gibt apt-listchanges einen Fehlercode zurück und APT bricht die Installation ab.

Nachdem apt-listchanges installiert wurde, zeigt es die Liste der Änderungen installierter Pakete an, wenn Pakete aus dem Netz (oder von einer CD oder gemounteten Partition) heruntergeladen werden, bevor sie installiert werden.

Das Arbeiten mit Quellpaketen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Herunterladen von Quellpaketen
""""""""""""""""""""""""""""""

In der Welt der freien Software ist es üblich, den Quellcode zu studieren oder auch Korrekturen an fehlerhaftem Code vorzunehmen. Um dieses zu tun, muss der Quellcode des Programms heruntergeladen werden. Das APT-System bietet eine einfache Möglichkeit, den Quellcode der vielen Programme der Distribution einschließlich aller für das Erstellen eines .deb des Programms nötigen Dateien zu beziehen.

Eine andere übliche Anwendung für Debian-Quellen ist es eine aktuellere Version eines Programms aus der Distribution unstable zum Beispiel in stable zu benutzen. Das Paket gegen stable zu kompilieren erzeugt ein Paket mit Abhängigkeiten, die auf die Pakete aus stable ausgerichtet sind.

Hierfür sollte der deb-src-Eintrag in Ihrer /etc/apt/sources.list auf unstable zeigen. Er sollte ausserdem aktiviert sein, d.h. eventuelle Kommentarzeichen vor der Zeile müssen entfernt werden (siehe Abschnitt  Die Datei /etc/apt/sources.list).

Um ein Quellpaket herunterzuladen, benutzen Sie folgendes Kommando:
root@linux # apt-get source Paketname

Drei Dateien werden daraufhin heruntergeladen: ein .orig.tar.gz, ein .dsc und ein .diff.gz. Im Falle von Paketen, die speziell für Debian erzeugt wurden, fällt das letzte weg und das erste hat kein orig im Namen.

Die Datei .dsc wird von dpkg-source benutzt, um das Quellpaket in das Verzeichnis Paketname-Version zu entpacken. In jedem heruntergeladenen Quellpaket befindet sich ein Verzeichnis debian/, welches die für das Bauen des .deb-Paketes nötigen Dateien enthält.

Um das Paket beim Herunterladen automatisch zu erzeugen, fügen Sie einfach -b zur Kommandozeile hinzu:
user@linux $ apt-get -b source Paketname

Wenn Sie sich dazu entscheiden, das Paket noch nicht beim Herunterladen zu erzeugen, können Sie dieses später nachholen mittels
user@linux $ dpkg-buildpackage -rfakeroot -uc -b

in dem Verzeichnis, welches für das Paket nach dem Herunterladen erstellt wurde. Um das zuvor erzeugte Paket zu installieren, muss man den Paketmanager direkt einsetzen:
root@linux # dpkg -i Datei.deb

Es besteht ein Unterschied zwischen der Methode apt-get source und den anderen Methoden von apt-get. Die source-Methode kann von normalen Benutzern ohne Root-Rechte benutzt werden. Die Dateien werden in das Verzeichnis heruntergeladen, aus dem das apt-get source Paket aufgerufen wurde.

Für das Kompilieren eines Quellpaketes nötige Pakete
""""""""""""""""""""""""""""""""""""""""""""""""""""

Normalerweise müssen sich spezielle Bibliotheken auf dem System befinden, um ein Quellpaket zu kompilieren. Alle Quellpakete haben ein Feld mit Namen Build-Depends in ihrer Kontrolldatei, welches die Namen der zusätzlichen Pakete enthält, die für das Erzeugen des Paketes aus dem Quellcode nötig sind.

APT bietet eine einfache Möglichkeit diese Pakete herunterzuladen. Führen Sie einfach apt-get build-dep Paket aus, wobei Paket für den Namen des Pakets, welches Sie erzeugen wollen steht. Beispiel:
root@linux # apt-get build-dep gmc
Reading Package Lists... Done
Building Dependency Tree... Done
The following NEW packages will be installed:
  comerr-dev e2fslibs-dev gdk-imlib-dev imlib-progs libgnome-dev
libgnorba-dev
  libgpmg1-dev
0 packages upgraded, 7 newly installed, 0 to remove and 1  not upgraded.
Need to get 1069kB of archives. After unpacking 3514kB will be used.
Do you want to continue? [Y/n]

Die Pakete, die hier installiert werden, werden gebraucht, um gmc korrekt zu erzeugen. Beachten Sie jedoch, dass dieses Kommando sich nicht um das Quellpaket selbst kümmert, welches Sie bauen möchten. Sie müssen hierfür zusätzlich apt-get source ausführen.

Falls Sie nur feststellen möchten, welche Pakete zum Bau eines bestimmten Paketes benötigt werden, ist eine Variante des Kommandos apt-cache show (siehe  Informationen über Pakete), die neben anderer Information die Zeile Build-Depends aufführt, die ihrerseits die erforderlichen Pakete auflistet.
root@linux # apt-cache showsrc Paket 

Der Umgang mit Fehlern
^^^^^^^^^^^^^^^^^^^^^^

Häufige Fehler
""""""""""""""

Fehler wird es immer geben. Viele werden durch unachtsame Benutzer verursacht. Im folgenden finden Sie eine Liste mit häufig gemeldeten Fehlern und wie Sie mit ihnen umgehen sollten.

Wenn Sie eine Nachricht erhalten, die aussieht wie die im unteren Beispiel, bei dem Versuch apt-get install Paket auszuführen ...
Reading Package Lists... Done
Building Dependency Tree... Done
W: Couldn't stat source package list 'http://people.debian.org unstable/ Packages' (/var/state/apt/lists/people.debian.org_%7ekov_debian_unstable_Packages) - stat (2 No such file or directory)
W: You may want to run apt-get update to correct these missing files
E: Couldn't find package penguineyes

haben Sie vergessen apt-get update nach Ihrer letzten Änderung in der /etc/apt/sources.list auszuführen.

Wenn folgender Fehler auftritt...
E: Could not open lock file /var/lib/dpkg/lock - open (13 Permission denied)
E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?

nach dem Versuch, irgend eine andere apt-get-Methode auszuführen als source, haben Sie keine Root-Rechte, d.h. Sie haben sie als normaler Benutzer ausgeführt.

Der gleiche Fehler wie oben tritt auf, wenn versucht wird, zweimal apt-get gleichzeitig auszuführen oder auch, wenn versucht wird apt-get auszuführen während ein dpkg-Prozess läuft. Die einzige Methode, die simultan zu anderen ausgeführt werden darf, ist die source-Methode.

Wenn eine Installation mitten im Prozess abbricht und Sie merken, dass es nicht länger möglich ist, Pakete zu installieren oder zu entfernen, versuchen Sie diese zwei Befehle auszuführen:
root@linux # apt-get -f install dpkg --configure -a

Danach versuchen Sie es erneut. Es kann nötig sein, den zweiten der beiden Befehle mehr als einmal auszuführen. Dies ist eine wichtige Information für die Abenteurer, die unstable benutzen.

Tritt E: Dynamic MMap ran out of room beim Ausführen von apt-get update auf, sollte die folgende Zeile der /etc/apt/apt.conf hinzugefügt werden:
/etc/apt/apt.conf

APT::Cache-Limit 10000000;


Wo gibt es Hilfe?
"""""""""""""""""

Wenn Sie sich von Zweifeln geplagt fühlen, ziehen Sie die umfangreiche Dokumentation des Debian-Paketsystems zu Rate. --help und Manpages können eine enorme Hilfe sein, genau wie die Dokumentation in den Verzeichnissen in /usr/share/doc ebenso wie in /usr/share/doc/apt.

Wenn diese Dokumentation nicht ausreicht, um Ihre Probleme zu beseitigen, versuchen Sie es auf den Debian-Mailinglisten. Mehr Informationen über die speziellen Benutzer-Listen gibt es auf der Debian-Webseite: de http://www.debian.org.

Natürlich sind diese Listen und Hilfen nur für Debian-Benutzer; Benutzer anderer Systeme werden von der Gemeinschaft ihrer Distribution bessere Hilfe erlangen. 

Welche Distributionen unterstützen APT?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Hier finden Sie die Namen einiger Distributionen, die APT unterstützen:

    Debian GNU/Linux (de http://www.debian.org) - Für diese Distribution wurde APT entwickelt
    Debian-Derivate wie (K)Ubuntu (en http://www.ubuntu.com, en http://www.kubuntu.com)
    Conectiva (en http://www.conectiva.com.br) - Die erste Distribution, die APT mit RPM benutzt
    Mandrake (en http://www.mandrake.com)
    PLD (en http://www.pld.org.pl)
    Vine (en http://www.vinelinux.org)
    APT4RPM (en http://apt4rpm.sf.net)
    Alt Linux (en http://www.altlinux.ru/)
    Red Hat (en http://www.redhat.com/)
    Sun Solaris (en http://www.sun.com/)
    SuSE (de http://www.suse.de/)
    Yellow Dog Linux (en http://www.yellowdoglinux.com/)
    Yoper (de http://www.yoper.de/, en http://www.yoper.com/)

Autoren

    Florian Frank florian.frank@pingos.org
    David Spreen netzwurm@debian.org
    Gustavo Noronha Silva kov@debian.org
	
Formatierung

    Florian Frank florian.frank@pingos.org


Richtlinien zur Systemverwaltung
--------------------------------

Richtlinien und Grundsätze
^^^^^^^^^^^^^^^^^^^^^^^^^^

Dieser kurze Text geht auf die wichtigsten Grundsätze der Systemverwaltung ein. Es sind allgemein gültige Grundsätze und Richtlinien, die jedem  Administrator bekannt sein sollten.

Auch Administratoren von Heimrechnern sollten die wichtigsten Punkte verinnerlichen. Auch bei solchen Rechnern schmerzt der Verlust der Daten...

Sollte eine wichtige Richtlinie fehlen, senden Sie uns bitte einige Zeilen dazu an unsere Adresse (  info@selflinux.org). Wir werden Ihren Vorschlag prüfen und gegebenenfalls in das nächste Release von SelfLinux aufnehmen.

    Den  root-Account nur so lange verwenden, wie unbedingt notwendig. Keinesfalls root für die tägliche Arbeit verwenden. Hierfür sollten ausschließlich die gewöhnlichen Benutzeraccounts genutzt werden.
    Administrative Aufgaben sollten niemals müde oder in Eile ausgeführt werden.
    Zu jeder Maschine sollte ein Maschinenbuch mit Einträgen über aufgetretene Probleme, einer Liste aller wichtigen Dateien (und deren Veränderung), einer Liste der Backups etc. geführt werden.
    "Never touch a running system" ist out. Security-Updates müssen so bald als möglich eingespielt werden. Um immer auf dem laufenden zu sein, empfiehlt sich das Lesen der Nachrichten von de DFN-CERT. Deren Sicherheits-Infos werden auf zahlreichen Webseiten angeboten.
    Immer an die  Backups denken. Backup-Strategien schon vor dem Systemausbau planen. Zudem dafür sorgen, dass Backups nicht nur erstellt, sondern auch geprüft werden.
    Bei wichtigen Systemen nicht nur ans Backup denken, sondern auch von vornherein einen Desaster-Recovery-Plan anfertigen.
    Größere Veränderungen an einer Maschine besser zuerst an einer baugleichen Maschine testen, sofern dies möglich ist.
    Nur Software installieren, die wirklich benötigt wird. Weniger Software bedeutet weniger potentielle Sicherheitslöcher.
    Auf einem Multiusersystem die Benutzer über Veränderungen schnellstens informieren. Nur informierte Benutzer können sich an neue Regeln halten.
    Regelmäßiges Sichten von  Logfiles. Vorkehrungen treffen, dass bei Unregelmäßigkeiten schnell gehandelt werden kann.
    Sichere  Passworte verwenden. Keine nebeneinander liegenden Tastenkombinationen (wie asdf oder qwer), Vornamen oder Geburtstage verwenden.

	
Autoren

    Johnny Graber linux@jgraber.ch
    Florian Frank florianfrank@gmx.de
    M. Kleine kleine_matthias@gmx.de
	
Formatierung

    Johnny Graber linux@jgraber.ch


Benutzerverwaltung unter Linux
------------------------------

Einleitung
^^^^^^^^^^

Dieses Kapitel führt in das Benutzerkonzept von Linux ein. Die Benutzerverwaltung sowie alle daraus resultierenden Aufgaben werden in späteren Kapiteln ausführlich behandelt. Die Grundlagen zum Benutzer- und Berechtigungskonzepte unter Linux werden in  diesem Kapitel erklärt.

Grundlagen
^^^^^^^^^^

Zu den wichtigsten Eigenschaften von Linux zählt sicher die Fähigkeit zu echtem Multitasking und zum Multiuser-Betrieb. "Multitasking" bedeutet dabei, dass mehrere Prozesse (englisch "tasks") parallel ablaufen, und sich dabei die vorhandenen Ressourcen teilen. "Multiuser" bedeutet, dass mehrere Benutzer gleichzeitig am System arbeiten und damit die Multitasking-Fähigkeiten des Systems nutzen können. Hierbei trennt das Betriebssystem die einzelnen Benutzer, die von diesen Benutzern gestarteten Prozesse/Tasks sowie die von ihnen angelegten  Dateien und  Verzeichnisse voneinander und kontrolliert restriktiv den Zugriff darauf.

In der  Windows-Welt ist dieser Multiuser-Betrieb wenig bekannt. Zwar ist es auch unter Windows z. B. möglich, mehrere  Benutzer auf demselben System anzulegen. Diese können im Normalfall aber nicht gleichzeitig an diesem System arbeiten.

Vorteile von Multiuser-Systemen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Für Privatanwender scheint ein Multiuser-System zunächst wenig vorteilhaft zu sein. In erster Linie denkt man bei solchen Systemen sicher an Server, auf die über Shell-Accounts, FTP, Internet usw. viele Kunden gleichzeitig zugreifen können. Trotzdem bringt dieses Konzept auch für "normale" Anwender entscheidende Vorteile:

    Ein Multiuser-System ist eine gute Basis für diszipliniertes Arbeiten. Man kann z. B. sehr genau beobachten, unter welchem Account man welche Aufgaben erledigt.
    Es bietet eine wesentlich höhere  Sicherheit als ein "Single-User-System", da die Zugriffsrechte auf Daten, Prozesse etc. je nach Benutzer und Nutzergruppe kontrolliert und eingeschränkt werden können.
    Dadurch ist es z. B. möglich, dass ein  Virus nur Zugriff auf die Daten desjenigen Benutzers erhält, über den er in das System gelangt ist. Voraussetzung dafür ist natürlich das bereits genannte disziplinierte Arbeiten.


Das Benutzerkonzept von Linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Unter Linux werden alle Benutzer, die zur Arbeit am System berechtigt sein sollen, zu Benutzergruppen zusammengefasst. Alle Ressourcen können nun für bestimmte Gruppen oder Benutzer freigegeben werden. Diese Rechtevergabe beruht auf:

    User-ID (uid)
    Dies ist die ID, mit der sich ein Benutzer am System anmeldet.
    effektive User-ID (euid)
    Dies ist die ID, mit der ein Benutzer auf eine Ressource zuzugreifen versucht.
    Group-ID (gid)
    Dies ist die ID, mit der sich eine Gruppe am System anmeldet.
    effektive Group-ID (egid)
    Dies ist die ID, mit der eine Gruppe auf eine Ressource zuzugreifen versucht.

Für die Zugriffskontrolle überprüft das System bei jedem Zugriff auf eine Ressource, unter welcher effektiven uid und guid der Zugriff erfolgt. Durch diese Unterscheidung von ID und effektiver ID ist es z. B. möglich, dass ein Benutzer zwar unter seiner eigenen uid arbeitet, zugleich aber (temporär) auf Dateien zugreift, die anderen Gruppen gehören.

Beispiel:

    Wird das  Passwort eines Benutzers geändert, so muss das Programm passwd z. B. auf die Datei /etc/shadow zugreifen. Normalerweise ist kein Benutzer berechtigt, auf diese Datei zuzugreifen. Trotzdem kann das Programm passwd diese Datei lesen, da es während dieses Vorganges einmal von der uid des aufrufenden Benutzers zur uid von root wechselt und damit über alle notwendigen Rechte verfügt. Dies ist möglich, da bei der ausführbaren Datei /usr/bin/passwd das Bit "set uid" gesetzt ist (in der folgenden Codezeile wird dies durch das erste "s" angezeigt): 

user@linux ~/temp$ ls -al /usr/bin/passwd
-rwsr-xr-x    1 root     root        24680 Apr  7 17:59

Weitere Informationen hierzu finden Sie mit "info chmod" und "man chmod". Mehr Information zu man und info finden Sie im Kapitel  Linux Hilfe. 

Wichtige Befehle
^^^^^^^^^^^^^^^^

ls -al - Anzeigen von Datei- und Verzeichniseigenschaften
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Beispiel:
user@linux ~/temp$ ls -al
total 8
drwxr-xr-x    2 hede     hede         4096 Jul 10 07:25 .
drwxr-xr-x    8 hede     hede         4096 Jul 10 07:25 ..
-rw-r--r--    1 hede     hede            0 Jul 10 07:25 test.txt

Die Datei test.txt gehört dem Benutzer hede und der Gruppe hede. Der Eigentümer hat Lese- und Schreibzugriff auf die Datei, während die Gruppe und alle anderen Benutzer die Datei nur lesen dürfen.

chmod - Ändern der Dateizugriffsrechte
""""""""""""""""""""""""""""""""""""""

Beispiel:

    Das folgende Shellkommando bestimmt, dass alle Mitglieder der Gruppe auch Schreibzugriff auf die Datei erhalten und andere Benutzer sie nicht lesen dürfen: 

user@linux ~/temp$ chmod 660 test.txt

Überprüfung:
user@linux ~/temp$ ls -al
total 8
drwxr-xr-x    2 hede     hede         4096 Jul 10 07:25 .
drwxr-xr-x    8 hede     hede         4096 Jul 10 07:25 ..
-rw-rw----    1 hede     hede            0 Jul 10 07:25 test.txt

chown - Ändern des Eigentümers und der Gruppenzugehörigkeit von Dateien und Verzeichnisse
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Test:
user@linux ~/temp$ ls -al
total 8
drwxr-xr-x    2 hede     users         4096 Jul 10 07:25 .
drwxr-xr-x    8 hede     users         4096 Jul 10 07:25 ..
-rw-rw----      1 user      users               0 Jul 10 07:25 test.txt

Die folgende Codezeile ändert den Besitzer der Datei:
user@linux ~/temp$ chown hede.users test.txt
user@linux ~/temp$ ls -al
total 8
drwxr-xr-x    2 hede     users         4096 Jul 10 07:25 .
drwxr-xr-x    8 hede     users         4096 Jul 10 07:25 ..
-rw-rw----      1 hede     users               0 Jul 10 07:25 test.txt

Um den Eigentümer und die Gruppe zu ändern, braucht man ausreichende  Rechte. Meist müssen derartige Änderungen also vom  root-Benutzer vorgenommen werden. Grundsätzlich können sie nur von einem Angehörigen derjenigen Gruppe durchgeführt werden, die zum Eigentümer der Datei erklärt werden soll.

chgrp - Ändern der Gruppenzugehörigkeit von Dateien oder Verzeichnissen
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Auch mit diesem Befehl kann man die Gruppenzugehörigkeit von Dateien und Verzeichnissen ändern. Soll nun die Gruppe users der Eigentümer der Datei test.txt werden, geht man folgendermaßen vor:
user@linux ~/temp$ chgrp users test.txt

Nachweis:
user@linux ~/temp$ ls -al
total 8
drwxr-xr-x     2 hede      hede          4096 Jul 10 07:25 .
drwxr-xr-x     8 hede      hede          4096 Jul 10 07:25 ..
-rw-rw----     1 hede      users            0 Jul 10 07:25 test.txt

Auch diese Änderung kann nur von einem Benutzer vorgenommen werden, der über die erforderlichen  Rechte verfügt.

id - Ermitteln der effektiven UIDs und GIDs
"""""""""""""""""""""""""""""""""""""""""""

Mit dem Befehl "id" kann man ermitteln, unter welcher UID man eingeloggt ist, zu welchen Gruppen man gehört, welche die primäre Gruppe ist usw.:
user@linux ~/temp$ id
uid=1000(hede) gid=1000(hede)
groups=1000(hede),25(floppy),100(users)

groups - die Gruppenzugehörigkeit ermitteln
"""""""""""""""""""""""""""""""""""""""""""

Mit dem Befehl groups kann man anzeigen lassen, welchen Gruppen man angehört:
user@linux ~/temp$ groups
hede floppy users

Diese Informationen kann man auch für andere Benutzer anzeigen lassen:
user@linux ~/temp$ groups root
root : root

Angemeldete Benutzer ermitteln
""""""""""""""""""""""""""""""

Der folgende Befehl zeigt an, wer momentan beim System angemeldet ist:
user@linux ~/temp$ w
07:37:58 up 15 days, 49 min,  5 users, load average: 0.05, 0.08 0.03
USER   TTY      FROM      LOGIN@   IDLE   JCPU   PCPU   WHAT
root   tty1     -         25Jun02  6:20   0.82s  0.82s  -bash
hede   tty2     -         02Jul02  7days  0.30s  0.30s  -bash
hede   tty3     -         Thu16    5days  0.21s  0.21s  -bash
hede   pts/0    www2      Mon11    3.00s  3.94s  3.45s  vim t.txt
hede   pts/1    www2      07:32    0.00s  0.13s  0.06s  w

Damit kann man also auch erkennen, wer sich von wo aus angemeldet hat und was die einzelnen Benutzer gerade machen.

Eine verkürzte Information liefert der Befehl who:
user@linux ~/temp$ who
root     tty1         Jun 25 06:49
hede     tty2         Jul  2 07:47
hede     tty3         Jul  4 16:28
hede     pts/0        Jul  8 11:45 (www2.sentec-elektronik.de)
hede     pts/1        Jul 10 07:32 (www2.sentec-elektronik.de)

Um zu ermitteln, unter welcher ID man selbst angemeldet ist, gibt man den Befehl whoami ein. Dieser zeigt aktuelle uid an:
user@linux ~/temp$ hede@www:~/temp$ whoami
hede

Informationen über laufende Prozesse, offene Dateien etc. ermitteln
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Mit "lsof" ("list open files") kann man herausfinden, welche Dateien im Moment geöffnet sind, und wer sie geöffnet hat.

"ps aux" zeigt die laufenden Prozesse sowie zusätzliche Informationen über Eigentümer, Ressourcenverbrauch usw. an.

Benutzerverwaltung
""""""""""""""""""

Die folgenden Befehle dienen der Benutzerverwaltung, also dem Anlegen, Ändern und Löschen von Benutzern und Gruppen:
useradd 	Neuen Benutzer anlegen
usermod 	Existierenden Benutzer ändern
groupadd 	Gruppe anlegen
groupmod 	Existierende Gruppe ändern

Diese Befehle werden in einem eigenen Kapitel ausführlich behandelt.

Die einzelnen Distributionen bieten darüber hinaus noch weitere Möglichkeiten, diese Verwaltungsaufgaben zu erledigen. So stellt z. B. de Debian die Tools adduser, addgroup, deluser, delgroup bereit. Unter de SuSE Linux kann man diese Aufgaben auch bequem mit yast erledigen. 

Hinweise
^^^^^^^^

Hier folgen noch ein paar Hinweise zur Arbeit mit dem Benutzerkonzept:

    Man sollte grundsätzlich nur dann als root arbeiten, wenn es unbedingt notwendig ist.
    Dadurch wird gewährleistet, dass man nicht aus Versehen wichtige Dateien löscht. Alle Prozesse (also z.B. auch Mail-Tools), die unter dem root-Account laufen, können uneingeschränkt auf das ganze System zugreifen. Tritt dabei ein Fehler auf oder enthält das Programm z. B. potenziell schädlichen Code, so ist das gesamte System gefährdet!
    Alle Dienste und Server sollten mit möglichst wenigen Rechten betrieben werden.
    Dieses Verhalten ist bei den meisten Systemen als Standard voreingestellt.
    Als Benutzer sollte man sich gut überlegen, welche Zugriffsrechte man Dateien und Verzeichnissen gibt.
    Mit einer behutsamen Rechtevergabe schützt man nicht nur die eigene Privatssphäre, sondern auch die Sicherheit des Systems.

Autor

    Heiko Degenhardt hede@pingos.org
	
Formatierung

    Matthias Hagedorn matthias.hagedorn@selflinux.org


Backups
-------

Beschreibung

Das Anlegen eines Backups sollte man nicht so lange hinauszögern, bis man es brauchen könnte. In dem Fall ist es zu spät und man hat nichts mehr, das sich sichern lässt. Dieser Text soll einige grundlegende Begriffe erklären und einem Ermöglichen zu entscheiden, was gesichert werden muss und was nicht.

Begriffe
^^^^^^^^

Backup
""""""

Mit Backup wird die Datensicherung bezeichnet. Der Sinn dieser Sicherung ist es, in möglichst kurzer Zeit wieder über all seine Daten zu verfügen, sollten diese durch einen äusseren Einfluss, einen Bedienungs- oder Programmfehler zerstört worden sein.
Man muss sich schon vorher Gedanken über die Datensicherung machen, denn wenn man das Backup brauchen könnte, ist es bekanntlich zu spät für eine Sicherung.

Vollständiges Backup
""""""""""""""""""""

Bei einem vollständigen Backup werden alle zu sichernden Daten auf einmal gesichert. Der Vorteil liegt darin, das all diese Daten in einem Durchgang zurückgespielt werden können, sollte dies notwendig werden.
Bei den heutigen Standardinstallationen der Distributionen wird man aber das Problem haben, sein ganzes System auf ein Medium zu schreiben. Einen Streamer haben die wenigsten Heimanwender und auf eine CD oder DVD passt ein komplettes System selten.

Zudem dauert ein vollständiges Backup ziemlich lange. Da man meist ungeduldig ist, kommt die lange Dauer des Backups als Ausrede gerade recht. Wieso sollte man auch täglich unzählige Dateien speichern, die man schon seit Wochen nicht mehr angefasst hat?

Differentielles Backup
""""""""""""""""""""""

Bei einem differentiellen Backup werden nur die seit dem letzten vollständigen Backup geänderten Dateien gesichert. Dies spart sowohl Zeit als auch Speicherplatz.

Bei einer Wiederherstellung der Daten muss man aber mit mehreren Medien arbeiten. Einerseits das vollständige Backup einspielen und danach noch das differentielle Backup.

Inkrementelles Backup
"""""""""""""""""""""

Beim inkrementellen Backup werden nur die Dateien gespeichert, die sich seit dem letzten Backup, geändert haben. Daher eignet sich das inkrementelle Backup besonders für die tägliche Sicherung.

Backup-Medien
"""""""""""""

Als Backup-Medium kann man alles verwenden, auf das man Daten speichern kann. Dies reicht von der Floppy-Diskette über Zips, CDs, DVD, Magnetbänder bis hin zu Festplatten. Schafft man sich ein neues Backup System an, sollte man sich vorher Gedanken über die notwendige Kapazität machen. Dadurch erspart man sich einen Fehlkauf und hat auch beim Betrieb weniger Ärger. Jedes mal für ein paar Dateien extra noch ein weiteres Medium anfangen nervt einen sehr bald. 

Backup-Strategien
^^^^^^^^^^^^^^^^^

Was soll gesichert werden?
""""""""""""""""""""""""""

Schlussendlich will man bei einem Datenverlust sein System möglichst schnell wieder herstellen können. Daher ergibt es sich, das man alle persönlichen Daten und Einstellungen sichern muss. Arbeitet man an Projekten oder mit Datenbanken, muss man diese auch ins Backup mit einbeziehen.

Was braucht man nicht zu sichern?
"""""""""""""""""""""""""""""""""

Es muss nichts gesichert werden, das man schon auf anderen CDs hat. Es macht wenig Sinn, bei jedem vollen Backup auch noch alle Programme, die mit der Distribution mitgeliefert wurden, zu sichern.
Braucht man für seine Arbeit aber zusätzliche Programme oder hat diese selber modifiziert, sollte man diese neben dem Backup auf eine Spezielle CD sichern. Diese Programme ändern sich ja kaum und wenn sie einmal gesichert sind, kann man diese wieder herstellen.

Wie geht man vor? Ein Beispiel
""""""""""""""""""""""""""""""

Wir nehmen an, dass wir 5 Dateien haben (a, b, c, d und e) und uns 8 CD-RWs zur Verfügung stehen.

Am Montag machen wir ein  volles Backup und sichern dabei alle Dateien auf unsere CD1.
Am Dienstag ändern wir die Dateien b, c und d. Da unsere Arbeit sehr wichtig ist, machen wir ein  differentielles Backup und sichern dabei die Dateien b, c und d auf die CD2.
Am Mittwoch ändern wir die Dateien d und e. Würden wir nun ein weiteres  differentielles Backup machen, müssten wir die Dateien b, c, d und e sichern, da sich diese seit dem vollständigen Backup geändert haben. Machen wir stattdessen ein  inkrementelles Backup, sichern wir nur die Differenzen zum letzten Backup - also die Dateien d und e auf CD3.

Mit dem  inkrementellen Backup fahren wir nun täglich weiter und fangen jeden Tag eine neue CD an, bis wir am nächsten Montag wieder ein volles Backup machen. Wichtig ist dabei, das wir dies auf die CD8 und nicht auf die CD1 machen. Geht beim Backup etwas schief, ist sonst unser volles Backup zerstört und alle unsere anderen CDs nutzen uns nichts mehr.

Sobald die CD8 korrekt geschrieben wurde, sind die CDs 1-7 nicht mehr notwendig um das System wieder aufzusetzen. CD1 sollte man vorerst bei Seite legen und erst wieder für das nächste volle Backup verwenden. CD 2-7 können für die inkrementellen Backups der nächsten Tage verwendet werden und helfen dabei beim Einsparen von Kosten.

So machen wir an den ersten 8 Tagen 8 Backups und brauchen 8 CDs. Dies mag auf den ersten Blick übertrieben erscheinen. Aber wie viel kostet ein Rohling? Wie wichtig ist die Arbeit und wie viel Zeit steckt dahinter?
Berechnet man die Kosten eines Datenverlustes (Erstellungszeit * Stundensatz), überwiegt diese Summe um Längen den Preis von 8 CD-RWs. Zumal diese 8 CDs ja wiederverwendet werden können.

komprimierte Archive
""""""""""""""""""""

Man stellt sich bald einmal die Frage, ob man die Archive nicht komprimieren könnte. Dadurch spart man zwar Platz, doch wird das Archiv anfälliger für Fehler.
Werden beim Schreiben des komprimierten Archivs nur wenige Bits falsch kopiert, kann das ganze Archiv unbrauchbar werden. Daher sollte man sicher sein, das beim Schreiben keine Fehler passieren. 

tar
^^^

Mit tar können einzelne Dateien oder ganze Verzeichnisse in ein ein Archiv gepackt werden. Da tar für grössere Systeme geschrieben wurde, versucht es standardmässig auf ein Bandlaufwerk zu schreiben.
Dies stellt kein grosses Problem dar, da man mit der Option -f den Datenstrom in eine Datei (f für File) umleiten kann. Daher wird im Verlauf des Textes tar immer auch mit -f aufgerufen.

tar ist zwar ein älteres Programm, doch ist es sehr weit verbreitet und unter vielen Systemen verfügbar. So kann man davon ausgehen, tar immer zur Verfügung zu haben.

Vollständiges Backup mit tar
""""""""""""""""""""""""""""

user@linux $ tar [Optionen] [Ziel] [Dateien und Verzeichnisse]

Will man eine neue Datei erstellen, genügen die Optionen -cf. Dabei werden aber die vorangestellten / der Pfadangaben entfernt. Will man die Dateien im Rahmen eines Restore zurückschreiben, braucht man aber die ganzen Pfade. Diese müssen beim Restore entweder angegeben werden oder beim Erstellen durch die Option -P explizit angefordert werden:
user@linux $ tar -Pcf /mnt/disk2/backup-1.tar /home/

Mit diesem Aufruf werden alle Dateien unter /home in die Datei /mnt/disk2/backup-1.tar gespeichert. Die gesamten Pfade bleiben erhalten, doch werden diese relativ von dem Ort aus eingetragen, von dem man tar aufruft.
Hinweis:
Durch die Verwendung von -P kann man die Dateien nur an ihren ursprünglichen Ort wiederherstellen. Will man sich alle Möglichkeiten offen halten, sollte man daher auf -P verzichten.

Inkrementelles Backup mit tar
"""""""""""""""""""""""""""""

tar wurde vor allem für ganze Backups gemacht. Es gibt aber einige mehr oder weniger einfache Möglichkeiten, inkrementelle Backups zu machen.

Die einfachste Form ist wohl die Verwendung der Option -g beim ersten vollen Backup.
Dabei werden Informationen über die gesicherten Dateien in eine externe Datei zeitstempel geschrieben:
user@linux $ tar -vcf /mnt/disk2/backup-1.tar -g zeitstempel /home/

Beim inkrementellen Backup lautet der Aufruf bis auf den Archivnamen gleich, doch sichert tar diesmal nur die Dateien, die sich seit dem vollen Backup geändert haben.

Man kann tar auch mitteilen, das es nur Dateien sichern soll, die sich seit einem bestimmten Datum geändert haben. Bei diesem Aufruf werden die seit dem 1. November modifizierten Dateien getart:
user@linux $ tar -N 2002-11-01 -Pcf /mnt/disk2/backup-1.tar /home/

Will man ein eigenes kleines Backupscript schreiben, das einem alle Dateien sichert, die neuer als 5 Tage sind, kann man date zur Mitarbeit bewegen:
user@linux $ tar -N $(date -d "now 5 days ago" +%Y-%b-%d) -Pcf /mnt/disk2/backup-1.tar /home/

komprimierte Backups mit tar
""""""""""""""""""""""""""""

Man kann ein nach dem zuvor erklärten Verfahren erstelltes Archiv auch komprimieren. Dazu genügt der Aufruf von gzip:
user@linux $ gzip backup-1.tar

Danach befindet sich im Verzeichnis eine Datei mit dem Namen backup-1.tar.gz. Um die Datei zu dekomprimieren, ruft man an der Stelle von gzip das Tool gunzip auf:
user@linux $ gunzip backup-1.tar.gz

Es gibt bei tar aber auch eine integrierte Methode. Mit der Option -z wird gzip durch tar aufgerufen und es gibt keinen Umweg über einen zweiten Befehl. Neben gzip kann auch bzip2 für die Komprimierung verwendet werden. Dafür dient die Option -j.
user@linux $ tar -zcf /mnt/disk2/backup-1.tar /home/
user@linux $ ls
backup-1.tar.gz

Überprüfen mit tar
""""""""""""""""""

Eine der wichtigsten Schritte beim Anlegen eines Backups ist dessen Überprüfung. Mit der Option -d geht dies sehr einfach, sofern man noch in dem Verzeichnis ist, in dem man tar gestartet hatte, keine Komprimierung -z oder -j und die Option -P verwendet hat:
user@linux $ tar -df /mnt/disk2/backup-1.tar

Wenn alles stimmt, gibt tar keine Fehlermeldung aus. Hat man -P nicht verwendet, muss man tar den Pfad mitgeben:
user@linux $ tar -C /home -df /mnt/disk2/backup-1.tar

Diese Option geht ebenfalls, wenn man zwischenzeitlich das Verzeichnis gewechselt hat.
Will man wissen, was alles im Archiv ist, genügt ein -t
user@linux $ tar -tf /mnt/disk2/backup-1.tar
/home/jg/datei1
/home/jg/datei2
/home/jg/datei3
/home/jg/datei4

Restore mit tar
"""""""""""""""

Nachdem das Archiv ordentlich geschrieben und geprüft wurde, kann man sich erst einmal ruhig zurücklehnen. Da aber erfahrungsgemäss immer etwas passieren kann, sollte man auch etwas über die Wiederherstellung wissen. Die dafür notwendige Option ist -x (extract = extrahieren). Damit man weiss was gerade passiert, gibt es -v (verbose = wortreich):
user@linux $ tar -xvzf /mnt/disk2/backup-1.tar.gz

oder mit zusätzlicher Pfadangabe und einem nicht komprimierten Archiv:
user@linux $ tar -C /home -xvf /mnt/disk2/backup-1.tar

cpio
^^^^

cpio (copy in/out) ist tar sehr ähnlich, doch kann es im Gegensatz dazu mit beschädigten Archiven umgehen. So kann man den unbeschadeten Teil des Archivs meistens noch retten. Allerdings funktioniert cpio nur auf Festplatten mit dem Dateisystem ext2.

Vollständiges Backup mit cpio
"""""""""""""""""""""""""""""

Will man die zu sichernden Dateien an cpio übergeben, müssen diese nach einem speziellen Muster mitgeteilt werden. Pro übergebene Zeile erwartet cpio einen Dateinamen. Daher verwendet man am Besten eine Pipe:
user@linux $ ls *.tex | cpio -o > /mnt/disk2/backup-2

Bei dieser Befehlskette werden zuerst alle TeX-Dateien (Endung .tex) aufgelistet und an cpio übergeben. Mit der Option -o wird die Datei backup-2 erstellt.

Der Nachteil beim Aufruf über ls ist, das man keine Unterverzeichnisse sichern kann. Verwendet man stattdessen find, kann man dieses Problem mit der Option -depth umgehen:
user@linux $ find /home/jg/ -maxdepth 2 -depth | cpio -o > /mnt/disk2/backup-3

Inkrementelles Backup mit cpio
""""""""""""""""""""""""""""""

Mit find kann man sich nicht nur Verzeichnisebenen anzeigen lassen, sondern Dateien auch auf ihre letzte Modifikation prüfen. Daher genügt die Ergänzung des vorhin verwendeten Befehls um -mtime -5 um alle Dateien zu erhalten, die in den letzten 5 Tagen geändert wurden:
user@linux $ find /home/jg/ -mtime -5 -maxdepth 2 -depth | cpio -o > /mnt/disk2/backup-4

Für weitere Optionen von find empfielt sich ein Blick in man find.

Überprüfen mit cpio
"""""""""""""""""""

Ein Nachteil von cpio ist die fehlende direkte Überprüfung der gesicherten Dateien. Um einen wirklichen Vergleich zu machen, müsste man die Dateien in ein anderes Verzeichnis entpacken und mit Hilfe von diff sicherstellen, das die Dateien korrekt sind.

Es gibt aber wenigstens eine Möglichkeit, sich die Dateien im Archiv anzeigen zu lassen:
user@linux $ cpio -itvI /mnt/disk2/backup-2
-rw-r--r--    1 jg      jg           100 Nov  5 20:03 /home/jg/test1.tex
-rw-r--r--    1 jg      jg            91 Nov  1 19:24 /home/jg/test2.tex
-rw-r--r--    1 jg      jg           212 Nov  4 17:05 /home/jg/test3.tex
-rw-r--r--    1 jg      jg            69 Nov  5 15:38 /home/jg/test4.tex

Da die Option i das Archiv auspacken würde, setzt man um dies zu Verhindern ein t davor. Über I teilt man das Verzeichnis mit und dank v werden die Dateirechte ebenfalls ausgegeben.

Zurückspielen mit cpio
""""""""""""""""""""""

user@linux $ cpio -id %lt; /mnt/disk2/backup-2

Damit werden die Dateien aus dem Archiv an ihren Platz zurück kopiert, sofern die dort vorhandenen Dateien nicht identisch oder älter sind. 

Autor

    Johnny Graber selflinux@jgraber.ch
	
Formatierung

    Alexander Fischer Alexander.Fischer@SelfLinux.org


cron
----

Beschreibung

Linux bietet zwei verwandte Programme für das Automatisieren von Aufgaben: cron und at. Beide starten beim Booten und laufen als Daemons, so werden sie niemals beendet.

Cron ist für sich wiederholende Aufgaben zuständig, at für einmalig ablaufende.

Der cron Batchdaemon
^^^^^^^^^^^^^^^^^^^^

Linux bietet etliche Programme für das Automatisieren von Aufgaben. Ein Beispiel ist cron. Der Batchdaemon cron ist für Aufgaben zuständig, die automatisch zu festgelegten Zeiten stattfinden sollen. Er wird beim  Booten gestartet und im Normalbetrieb nicht beendet. Ein Daemon ist ein Prozess, der im Hintergrund des Systems läuft und von dem man normalerweise keine Ausgabe sieht.

cron liest eine sogenannte crontab-Datei, um Informationen für seine Arbeit zu erhalten. Es gibt eine systemweite und eventuell einige benutzerspezifische crontab-Dateien. Die Datei des Systems befindet sich unter /etc/crontab. Verändern Sie diese Datei auf keinen Fall, wenn Sie nicht genau wissen, was Sie tun. Selbst der Benutzer  root sollte seine eigene Datei erzeugen.

Erzeugen der crontab-Datei
^^^^^^^^^^^^^^^^^^^^^^^^^^

Prüfen Sie zunächst, ob für den Benutzer, dessen crontab-Datei Sie erzeugen möchten (in unserem Fall root), noch keine solche Datei existiert. Dies können Sie mittels
root@linux ~# crontab -l

tun. Es sollte die Ausgabe no crontab for root erscheinen. Andernfalls würde das folgende Kommando die alte crontab-Datei überschreiben. Geben Sie dann Folgendes ein:
root@linux ~# crontab /etc/crontab

Das Kommando wird eine crontab-Datei für Sie erzeugen, die eine Kopie der systemweiten crontab-Datei ist. Editieren Sie nun ihre Datei mittels
root@linux ~# crontab -e

Beachten Sie, dass crontab sowohl der Name der Datei als auch der Name des ausführbaren Programmes ist, ähnlich wie im Falle des Kommandos  passwd. Die erzeugte Datei wird in etwa wie diese Konfiguration aussehen (dies ist ein Beispiel einer RedHat-Datei):
Konfiguration RedHat-Datei

SHELL=/bin/bash
PATH=/sbin:/bin/:/usr/sbin/:/usr/bin
MAILTO=root
HOME=/

#run-parts
01 * * * * root run-parts /etc/cron.hourly
01 4 * * * root run-parts /etc/cron.daily
22 4 * * 0 root run-parts /etc/cron.weekly
42 4 1 * * root run-parts /etc/cron.monthly
    

Löschen Sie alle Zeilen nach der Zeile HOME=/. Dies sind nicht die Aktionen die Sie ausführen wollen. Ändern Sie außerdem die Variable PATH so, dass sie das Verzeichnis enthält, in dem Sie Ihre Skripte oder Programme speichern, die zeitgesteuert ablaufen sollen.

cron Konfiguration
^^^^^^^^^^^^^^^^^^

Jede Zeile in der crontab-Datei führt ein eigenes Programm aus. Die Zeilen haben ein spezielles Format:

Fünf Zeitfelder, gefolgt von dem auszuführenden Programm. Beachten Sie: In der systemweiten crontab-Datei ist ein weiteres Feld, das den Daemon anweist, das Programm als einen bestimmten  Benutzer auszuführen (z. B. root). In einer benutzerspezifischen crontab-Datei wird dieses Feld ignoriert.

Die fünf Zeitfelder sind:

Minuten Stunden Tag-des-Monats Monat Wochentag
Auszug aus der Manpage

Die Zeit- und Datumsfelder sind:

Minute           0-59
Stunde           0-23
Tag-des-Monats   1-31
Monat            1-12 (oder Namen, siehe unten)
Wochentag        0-7 (0 oder 7 ist Sonntag oder Namen)

Ein Feld kann ein Stern (*) sein, was immer für
"Erster-Letzter" steht.

Zahlenbereiche sind erlaubt. Bereiche sind zwei Zahlen,
getrennt durch einen Bindestrich. Die angegebenen Grenzen sind
inklusive.
Beispielsweise: 8-11 in "Stunde" bewirkt die Ausführung um 8,
9, 10, 11 Uhr.

Listen sind erlaubt. Eine Liste ist eine Menge von Nummern
(oder Bereichen), getrennt durch Kommata.
Beispiele: "1,2,5,9", "0-4,8-2". (Die Hochkommata nicht mit
in die Datei übernehmen, Anmerkung des Übersetzers)

Schrittweiten können in Verbindung mit Bereichen genutzt
werden. Hinter einem Bereich mit "/<Schrittweite>" angegeben,
bestimmt die Schrittweite, ob Werte innerhalb des Bereiches
übersprungen werden.
Beispiel: "0-23/2" kann unter Stunden benutzt werden, um ein
spezielles Kommando alle zwei Stunden auszuführen. Die
Alternative wäre:
"0,2,4,6,8,10,12,14,16,18,20,22". Schrittweiten sind auch nach
Sternen (*) erlaubt, "alle zwei Stunden" lässt sich auch durch
"*/2" beschreiben.

Namen können für "Monat" und "Wochentag" benutzt
werden. Benutzen Sie die ersten drei Buchstaben des
entsprechenden Tages oder Monats (Groß-/Kleinschreibung wird
nicht beachtet). Bereiche oder Listen sind mit Namen nicht
erlaubt.


Ein praktisches Beispiel
^^^^^^^^^^^^^^^^^^^^^^^^

Für ein Backup-Skript zur  Datensicherung, das um 5 Minuten nach 1 Uhr morgens jeden Tag laufen soll, würde die zugehörige crontab-Datei folgendermaßen aussehen:
Backup-Skript

SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin
MAILTO=root
HOME=/

5 1 * * * /usr/local/bin/run-backup
    

Durch die Angabe MAILTO=root teilt cron die Beendigung der Aufgabe dem Benutzer root per E-Mail mit. Ist man mit dem Ablauf des Skriptes run-backup zufrieden und der Benachrichtigungen überdrüssig, ändern man die MAILTO-Zeile folgendermaßen:
run-backup

MAILTO=""


Mehr Information
^^^^^^^^^^^^^^^^

Weitere Information findet sich in folgenden Manual-Seiten:

    man crontab
    man 5 crontab
    man cron

Autor

    JC Pollman jpollman@bigfoot.com
	
Formatierung

    Matthias Hagedorn matthias.hagedorn@selflinux.org



Syslog
------

Beschreibung

Syslog ist ein Dienst, der zentral Meldungen von anderen Diensten und Daemonen des Systems einsammelt, und beispielsweise in Logdateien schreibt, zum Beispiel nach /var/log/messages oder über das Netzwerk auf einen zentralen Logserver schickt.

Einleitung
^^^^^^^^^^

Auf einem System laufen, selbst wenn man momentan gerade nicht direkt damit arbeitet, stets viele Dienste. Bei typischen Linuxinstallationen sind es schnell einhundert Prozesse, die im Hintergrund ihre Arbeit verrichten. Auf einem Server ist "ständig was los", auch wenn man diesem das nicht unbedingt ansieht.

Hintergrundaktivitäten
""""""""""""""""""""""

Etliche dieser Hintergrundprozesse sind Dienste, Server oder Daemons. Alle drei Begriffe meinen im Wesentlichen das Gleiche.

Beispiele für solche Dienste sind FTP-, Web- und Mailserver. Da ein Dienst über keine direkte Benutzerschnittstelle verfügt (das heißt, er hat kein "Fenster"), kann er Meldungen und Fehler nicht direkt an den Benutzer melden. Oft sind die Benutzer, die gerade mit einem System arbeiten, auch die nicht gewünschten Empfänger, denn viele Meldungen sind eher für Systemadministratoren gedacht. Auf kleinen Systemen jedoch sind die Benutzer oft gleichzeitig Administratoren.

Um einen ordnungsgemäßen Betrieb zu gewährleisten, muß das System überwacht, und gegebenenfalls auf Fehler reagiert werden.

Auch der Linux-Kernel selbst erzeugt Meldungen, beispielsweise über Hardwarefehler (wie kaputte Festplatten).

Logdateien
""""""""""

Um derartige Meldungen zu verarbeiten, können diese beispielsweise in Logdateien geschrieben werden. Diese können dann von Zeit zu Zeit von einem Administrator geprüft werden, oder bei Fehleranalysen verwendet werden. Der Apache-Webserver schreibt beispielsweise solche Logdateien.

Dieser Weg ist aber für Meldungen von vielen kleineren Diensten umständlich, da man viele Logdateien in unterschiedlichen Formaten analysieren müßte: Zwanzig verschiedene Dienste würden zwanzig verschiedene Logdateien schreiben.

Hier gibt es jedoch einen Dienst, der dafür zuständig ist, von beliebigen Diensten Meldungen einzusammeln, und zentral in Logdateien zu schreiben. Der Dienst, der sehr häufig hierfür eingesetzt wird, ist Syslog.

Syslog - der System Logger
""""""""""""""""""""""""""

Syslog bietet anderen Diensten eine Schnittstelle, über die Meldungen übergeben werden. Syslog verarbeitet diese Meldungen dann weiter, in dem sie in Dateien geschrieben werden.

Dieser Dienst ist einfach zu handhaben und wird deshalb insbesondere von kleineren Diensten gerne verwendet. Diese Dienste müssen sich dann nicht alle einzeln um Logdateien kümmern. Für den Administrator hat dies Vorteile: Es gibt eine zentrale Konfigurationsdatei und zentrale Logdateien.

Zusätzlich erlaubt es Syslog auch, Logmeldungen an andere Server über das Netzwerk weiterzureichen. Auf dem Zielserver nimmt wiederum Syslog diese Nachrichten ab, und schreibt sie in Logdateien. Damit lassen sich die Nachrichten von mehreren Systemen auf einem Server zusammenfassen. 

Meldungen
^^^^^^^^^

Wenn ein Dienst den Syslog-Dienst verwendet, schickt er seine Meldungen also einfach an Syslog. Eine Meldung ist im Wesentlichen eine Textzeile. Zusätzlich enthält diese noch einige Statusinformationen, beispielsweise wie wichtig diese Meldung ist und zu welchem "Themengebiet" sie gehört bzw. von welcher Quelle sie kommt.

Syslog prüft anhand dieser Werte, ob und wie diese Meldung verarbeitet werden soll. Man kann Syslog zum Beispiel so konfigurieren, dass wichtige Meldungen in der einen und unwichtige in einer anderen Datei stehen, oder dass alle Meldungen, die vom Mailsystem kommen, auf einen anderen Rechner übertragen werden sollen und weiteres.

Quellen von Meldungen
"""""""""""""""""""""

Syslog definiert eine Reihe von Quellen oder Themengebieten für Meldungen. Diese werden als facilities bezeichnet. Dabei ist es nur eine Konvention, wann welche Facility verwendet wird. Manchmal ist eine eindeutige Zuordnung nicht einfach möglich. Hier muß man nachsehen, welche Facility ein Dienst benutzt (bei manchen Diensten läßt sie diese auch einstellen).

Die nachfolgende Übersicht beschreibt die Facilities kurz:
Name 	Bedeutung
auth, authpriv 	Meldungen, die zur Authentifizierung gehören, beispielsweise falsche Passwörter.
cron 	Meldungen, die von Cron erzeugt wurden, oder von Prozessen, die von Cron gestartet werden (die Standard-Ausgabe und Stardard-Fehler-Ausgabe werden jedoch von Cron nicht an Syslog gereicht, sondern per EMail verschickt).
daemon 	Meldungen von allgemeinen Diensten, wie zum Beispiel einem FTP-Server.
kern 	Meldungen des Systemkernels. Sollte von keinem Dienst verwendet werden. Hierzu gehören beispielsweise Hardware-bezogene Meldungen.
lpr 	Meldungen des Drucksystems
(Druckerspooler).
mail 	Meldungen des Mailsystems (beispielsweise von sendmail und fetchmail).
mark 	Nur für Syslog-interne Zwecke, sollte nie verwendet werden.
news 	Meldungen des News-Systems, zum Beispiel eines Newsservers.
syslog 	Meldungen von Syslog selbst.
user 	Meldungen von Benutzersystemen wie zum Beispiel eigenen Scripten.
uucp 	Meldungen von Unix-Unix-Copy (UUCP wird heute kaum noch verwendet).
local0 bis local7 	Diese sind frei und können nach Belieben verwendet werden. Bei Diensten, bei denen man die zu verwendende Facility einstellen kann, kann man diese verwenden und je nach Bedarf verteilen.

Priorität von Meldungen
"""""""""""""""""""""""

Syslog definiert eine Reihe von Namen, die die Wichtigkeit beschreiben. Diese Priorität wird englisch als priority oder severity bezeichnet. Manchmal spricht man auch vom log level. Auch diese Eigenschaft kann man verwenden, um die Meldungen differenziert zu verarbeiten, wie bereits angedeutet.

Die folgende Übersicht nennt die definierten Prioritäten:
Name 	Beschreibung
debug 	Unwichtige Meldungen, dienen nur zu Debug-Zwecken (Fehlerfindung vor allem bei der Entwicklung).
info 	Informative, nicht weiter wichtige Meldungen.
notice 	Informative Meldungen, die größere Bedeutung haben als info.
warning 	Warnungen, also Meldungen, die nicht-fatale Fehler anzeigen.
err 	Fehlermeldungen, die kleine Störungen anzeigen.
crit 	Kritische (schwerere) Fehler, die beispielsweise Teilausfälle anzeigen.
alert 	Schwere Fehler, die erhebliche Störungen und Ausfälle anzeigen.
emerg 	Sehr schwere Fehler, die beispielweise den Totalausfall des Systems anzeigen können und schwere Kernelfehler (Hardwareausfälle).

Als Faustregel gilt hier: Alles, was wichtiger oder gleich warning ist, verdient auf jeden Fall Aufmerksamkeit.

Weitere Eigenschaften von Meldungen
"""""""""""""""""""""""""""""""""""

Zu diesen beiden essentiellen Eigenschaften kommt die Information über den Zeitpunkt der Meldung hinzu (diese wird von Syslog automatisch beigefügt), die Prozeß-ID des Prozesses, der diese Meldung erzeugte, und ein Tag, das meistens den Namen des Programmes enthält, das die Meldung erzeugte (beispielsweise verwendet Sendmail das Tag sendmail). Auch der Hostname des Systems wird hinzugefügt, was insbesondere wichtig ist, wenn man von mehreren Systemen über das Netzwerk auf ein zentrales loggt.

Festes Format der Meldungen
"""""""""""""""""""""""""""

Beim Schreiben in Logdateien verwendet Syslog ein festes Format. Eine Meldung ist immer eine Zeile. Zu Beginn steht der Zeitstempel, dann folgt der Hostname des Systems. Das dritte Feld ist das Tag (also meistens der Programmname) und in eckigen Klammern dessen Prozeß-ID. Der Rest der Zeile ist die Textnachricht dieser Meldung. Bei einigen Meldungen weicht das Format geringfügig ab, beispielsweise kann die Prozeß-ID entfallen (wie etwa bei Kernel und syslog Meldungen).

Die Facility und Priority sind aus einer solchen Zeile leider nicht mehr erkennbar.

Ein Beispiel für eine Logmeldung:
/var/log/messages

 Mar 10 13:30:30 atlas syslogd 1.3-3: restart.  

Diese Meldung wurde am 10. März um 13:30 Uhr auf einem Host namens atlas erzeugt, und gibt an, dass Syslog gestartet wurde (diese Meldung erzeugt Syslog selbst beim Start).

Meldungen des Kernels
"""""""""""""""""""""

Der Kernel verschickt seine Meldungen auf eine etwas andere Art. Der Kernel kann sich nicht wie ein normales Programm verhalten, beispielsweise weil er als erstes läuft, kein normaler Prozeß ist, und weil er aus Performancegründen nicht auf die Fertigstellung von Schreiboperationen warten kann.

Der Kernel legt alle Meldungen in einem speziellen Speicherbereich ab. Diese kann man zunächst überhaupt nicht lesen. Es gibt nun einen speziellen Dienst, den Kernel-Logger klogd. Dieser Dienst holt die Meldungen aus dem Speicherbereich ab und wandelt sie auch in menschenlesbare Meldungen um. Der Kernel-Logger kann die Kernelmeldungen in eine Datei schreiben, oder an Syslog weiterversenden. Die zweite Möglichkeit ist die Voreinstellung. Startet man den Kernel-Logger, holt er die Kernelmeldungen sofort ab, und verschickt diese an Syslog. Syslog schreibt sie dann in eine Datei. Normalerweise wird klogd automatisch direkt nach syslogd gestartet. 

Konfiguration von Syslog
^^^^^^^^^^^^^^^^^^^^^^^^

Über eine zentrale Datei wird das Verhalten von Syslog gesteuert. Zusätzlich akzeptiert Syslog einige Kommandozeilen-Parameter, die das Verhalten beeinflussen.

Die Konfigurationsdatei
"""""""""""""""""""""""

Fast immer heißt diese Datei /etc/syslog.conf. Hier wird eingestellt, wie Meldungen in Dateien zu schreiben sind, bzw. wie diese über das Netzwerk übertragen werden.

Diese Datei ist zeilenorientiert. Zeilen, die mit einem Hashmark ("#") beginnen, sind Kommentare und werden ignoriert.

Zeilen bestehen aus zwei Teilen, die durch mindestens einen Tabstop getrennt sind (moderne GNU/Linux Syslog Implementierungen erlauben oft auch Leerzeichen, es sollten aber sicherheitshalber Tabstops verwendet werden). Zu Beginn der Zeile, also auf der linken Seite, steht eine Beschreibung der Nachricht. Hier können über die Facility und Priority Meldungen ausgewählt werden.

Der zweite Teil gibt dann an, was mit diesen ausgewählten Meldungen passieren soll. Hier steht meistens ein Dateiname. In diese Datei werden dann die Meldungen geschrieben. Es sind nicht nur Dateinamen erlaubt: NetzwerkAdressen (IP-Adressen oder Hostnamen) sind hier erlaubt und auch Benutzernamen können hier stehen. Im letzten Fall werden die Meldungen auf die Konsolen der entsprechenden Benutzer geschrieben. Das kann für sehr wichtige Meldungen Sinn machen, wird aber im Allgemeinen als störend empfunden. Oft verwendet man als einzigen Benutzernamen root, damit ein eventuell angemeldeter Administrator wichtige Meldungen sofort auf sein Terminal bekommt (insbesondere bei Störungen und der Suche nach den Ursachen für diese ist das oft hilfreich).

Es werden immer alle Aktionen ausgeführt, deren Beschreibung auf die Meldung paßt. Dadurch kann ein- und dieselbe Meldung in mehrere Dateien geschrieben und gleichzeitig beispielsweise über das Netzwerk verschickt werden.

Meldungsbeschreibung

Die Meldungsbeschreibung ist eine Liste aus Facility/Priority-Paaren. Das wird so verwendet: Ist eine Meldung der entsprechenden Facility mindestens so wichtig, wie die Priority angibt, wird die rechte Seite verwendet (die Aktion wird ausgeführt). Hat man viele Regeln, so werden wichtige Meldungen damit oft in mehrere Dateien geloggt; dies ist erwünscht.

Der Grundaufbau der durch Semikolon (;) getrennten Facility/Priority-Paaren ist einfach: Facility und Priority stehen in dieser Reihenfolge durch einen Punkt getrennt. Wichtige Kernelmeldungen lassen sich beispielsweise durch:

kern.warning

beschreiben. Hier sind nicht nur die Meldungen der Priorität warning, sondern auch alle höheren (also err, crit, alert und emerg) gemeint. Man kann auch Wildcards verwenden, so zum Beispiel *.warning für alle wichtigen Meldungen und kern.* für alle Kernelmeldungen. Hierbei ist jedoch zu beachten, daß nicht alle Syslogdienste Wildcard verstehen (aber die unter GNU/Linux verwendeten können dies). Bei GNU/Linux-Syslog kann man auch mehrere Facilities durch Komma (,) getrennt aufführen. Dies ist jedoch nur zulässig, wenn diese alle die gleiche Priorität haben (die nur an der letzten Facility angehängt steht). Nach dieser komplizierten Erklärung ein einfaches Beispiel: Wichtige Meldungen von News oder Mail:

news,mailwarning

Oft wird jedoch auch hier lieber eine durch Semikolon getrennte Liste verwendet, weil es als lesbarer und weniger verwirrend empfunden wird:

news.warning;mail.warning

Verwendet man Wildcards, kann man sämtliche Meldungen mit *.* erfassen. GNU/Linux-Syslog bietet neben Wildcards weitere nützliche Erweiterungen: Möchte man nicht, dass alle Meldungen auch höherer Priorität passen, kann man vor die Priority ein Gleichheitszeichen (=) setzen, beispielsweise *.=warning. In solchen Fällen muß man aber unbedingt Regeln haben, die auch die wichtigeren Meldungen verarbeiten, sonst fehlen am Ende die wichtigsten Meldungen!

Es ist auch möglich, mit einem Ausrufezeichen (!) eine Priorität auszuschließen, zum Beispiel *.=!warning, was man aber sehr selten sieht.

Widersprechen sich Bedingungen einer durch Semikolon getrennten Bedingungsliste, so gilt die zuletzt beschriebene, also

kern.=!info;kern.*

loggt jegliche Kernelmeldung (hier meinte man vermutlich einfach kern.=!info). Solche Konstrukte sind natürlich zu vermeiden.

Es gibt eine spezielle Priority none, die für keine Priority der dazugehörigen Facility steht:

*.info;mail.none

bezeichnet alle Meldungen mit Priority info, außer von der Facility mail.

Meldungsaktion

Auf der rechten Seite steht dann, was mit Meldungen geschehen soll. Im einfachsten Fall steht hier ein Dateiname. Diese müssen mit vollem Pfad angegeben werden, sie beginnen also mit einem Slash (/). Beispiel: /var/log/messages. Möchte man nicht-synchronisiert schreiben (später mehr dazu), schreibt man noch ein Minus (-) davor: -/var/log/messages. Als spezielle Datei kann man auch ein Terminal, zum Beispiel /dev/tty10, verwenden. Dann erscheinen die Meldungen auf der Konsole 10 (die man meist über ATL+F10 oder STRG+ALT+F10 erreicht).

Neben Dateien kann man auch sogenannte named FIFOs verwenden. Diese beginnen mit einem Pipezeichen (|), gefolgt vom Dateinamen. Für diese Spezialität erfolgt an dieser Stelle jedoch keine detaillierte Diskussion.

Soll die Meldung an einen anderen Server übertragen werden, schreibt man ein At-Zeichen (@) und den Hostnamen oder besser eine IP-Adresse des Systems, zum Beispiel @192.168.1.14.

Um die Meldung auf die Terminals von Benutzern zu schreiben, schreibt man einfach dessen Accountnamen, beispielsweise root. Hier darf auch ein * stehen. Dann werden alle Benutzer (also alle Terminals) informiert. Das kann jedoch stören, denn die Meldungen erscheinen dann "mitten im Terminaltext" und "verunstalten" das Layout der laufenden Anwendung (wenn man zum Beispiel gerade einen Editor offen hat. Etliche Anwendungen haben eine Möglichkeit, die Anzeige neuzuzeichnen, häufig STRG+L ).

Beispielkonfigurationsdatei

Es folgt ein Beispiel mit ausführlichen Kommentaren.
/etc/syslog.conf

 
#/etc/syslog.conf: Syslogkonfigurationsdatei
#Zur Trennung der "linken" und "rechten" Seite sollten
#   Tabstops verwendet werden (moderne GNU/Linux Syslog
#   Implementierungen kommen meist auch mit Leerzeichen klar)

#sehr wichtige Warnungen vom Kernel, und alle Fehler außer
#   evtl. Passwortvertipper auf die Konsole ALT-F10 loggen.
#   Zur Erinnerung: .warn schließt höhere Meldungen (also err,
#   crit, alert, emerg) mit ein. Diese landen also auch auf
#   ALT-F10.
kern.warning;*.err;authpriv.none        /dev/tty10

#Die gleichen Meldungen für die xconsole bereitstellen
#  (Hier wird ein FIFO verwendet, der von xconsole
#   ausgelesen wird)
kern.warn;*.err;authpriv.none           |/dev/xconsole

#ALLE Meldungen auf ALT-F9. Das ist für einen schnellen Überblick
#  bei "Echtzeit-Fehleranalyse" hilfreich
*.*                                     /dev/tty9

#alle sehr schweren Fehler direkt an alle Benutzer in die Konsolen
#  schreiben. In solchen Fällen ist das System vermutlich eh kaum
#  noch benutzbar. Eventuell sieht man aber kurz vor dem Absturz
#  noch eine Fehlermeldung und kann nach einem Neustart etwas
#  ändern
*.emerg                                 *

#Root möchte eventuell auch schon "crit" auf der Console sehen,
#  wenn er zufällig gerade an dem System arbeitet:
#*.crit                                 root

#Alle EMail-Meldungen in eine eigene Datei. Diese Datei wird
#  aus Performance-Gründen nicht nach jeder Zeile synchronisiert,
#  (aber schwere Fehler schreiben wir nochmal in eine extra Datei)
mail.*                                  -/var/log/mail

#Warnungen in eine extra Datei. Diese wird hier nicht sync'd (bei
#  langsameren Systemen hilfreich)
*.=warn;*.=err                          -/var/log/warn

#"crit" und höhere kommen in die gleiche Datei, aber sync'd
*.crit                                  /var/log/warn

#alles außer "debug" und mail in eine andere Datei
*.info;mail.none                        -/var/log/messages

#Für die Fehlersuche hilft oft eine Datei, die sämtliche
#Informationen enthält
#*.*                                    -/var/log/allmessages

#Hat man einen Loghost, soll dieser eine Kopie von allen
#  Meldungen erhalten
#*.*                                    @192.168.1.1

#Weniger wichtige Systeme sollen das Netzwerk nicht unnötig
#  belasten
#*.warn                                 @192.168.1.1
      


Kommandozeilenoptionen
""""""""""""""""""""""

Kommandozeilenoptionen steuern das Verhalten von Syslog ebenfalls. Über Kommandozeilenoptionen kann Syslog konfiguriert werden, dass Meldungen vom Netzwerk (also anderen System-Loggern) akzeptiert und verarbeitet werden. Man kann Syslog auch veranlassen, eine andere Konfigurationsdatei zu verwenden.
Option 	Beschreibung
-a <Socket> 	Öffnet <Socket> zum Lesen von Meldungen. <Socket> ist auf /dev/log voreingestellt. Hier kann zum Beispiel zusätzlich ein dev/log aus einer "chroot" Umgebung angegeben werden, damit die "chroot" Umgebung auch Syslog verwenden kann.
-d 	Debug Modus (für Entwickler gedacht)
-f <Konfigdatei> 	Lädt eine andere Konfigdatei. Normalerweise wird /etc/syslog.conf verwendet.
-h 	Über das Netzwerk empfangene Meldungen auch über das Netzwerk weitersenden. Damit kann man mehrere Netzwerk-Syslogs "in Reihe schalten", um Beispielsweise Meldungen durch mehrere Firewalls oder aus einer DMZ zu bekommen. -t sollte auch verwendet werden, siehe dort.
-l <Hostnamen> 	Eine durch : getrennte Liste von Hostnamen, die in kurzer Form in der Logdatei stehen. Gewöhnlich bevorzugt man die Option -s, die ähnliches Verhalten bringt.
-m <Mark Zeit> 	Syslog schreibt alle 20 Minuten einen Eintrag --MARK-- in ein Logfile. Daran kann man erkennen, dass das System noch lebt. Bei der nachträglichen Analyse kann man dadurch beispielsweise nächtliche Abstürze zeitlich eingrenzen. Durch diese Option kann man anstatt 20 (Minuten) auch einen anderen Wert verwenden. Der Wert 0 schaltet die Funktion ab.
-n 	Syslog soll nicht automatisch in den Hintergrund gehen. Diese Option wird im Normalfall nicht verwendet. Auf speziellen Systemen (Rettungs- oder Installationsystemen) wird diese manchmal gesetzt.
-p <Socket> 	Öffnet <Socket> zum Lesen von Meldungen. Siehe Option -a.
-r 	Aktiviert den Empfang von Netzwerkmeldungen. Aus Effizienz- und Stabilitätsgründen sollte man alle IPs, von denen man Meldungen empfängt, in die Datei /etc/hosts eintragen (diese werden benutzt, um den Hostnamen für das Logfile zu bilden)
-s <Domains> 	<Domains> ist eine durch : getrennte Liste von Domains, die vor dem Loggen von Hostnamen abgeschnitten werden. Das ist in Verbindung mit -r hilfreich, da die FQDNs (vollen Namen) viel Platz im Logfile wegnehmen, und die Hostnamen meistens sowieso schon eindeutig sind. Hat man einen host mail.selflinux.de und ein -s selflinux.de, so wird der Hostname also als mail in den Logdateien stehen.
-t 	Weitergeleitete Meldungen (siehe Option -h) sollen den empfangenen Hostnamen enthalten, nicht den eigenen. Das heißt also, der Hostname der Meldung wird nicht verändert; diese können damit weiterhin eindeutig zugeordnet werden.

Diese Optionen werden in der Regel vom Syslog-Startscript, häufig /etc/init.d/syslog, beim Start von Syslog an diesen übergeben. Hier kann man also die eigenen Optionen für den Aufruf angeben.

Bei SuSE-Systemen ist das Startscript intelligenter, es gestattet dem Administrator, auf einfachem Weg weiterere Startoptionen zu setzen. Hierzu öffnet man dazu einfach die Datei /etc/rc.config und ändert
/etc/rc.config

  SYSLOGD_PARAMS=""  

so, dass die erwünschten Startoptionen verwendet werden. Diese werden hier einfach eingetragen.

Auch unter RedHat muß man nicht mehr die Datei /etc/init.d/syslog bearbeiten, unter /etc/sysconfig/syslog kann man durch Ändern von SYSLOGD_OPTIONS="" (z.B. "-r -m 0 -s picard.inka.de:zeibig.net") die gewünschten Startoptionen angeben.

Remote-Logging
""""""""""""""

Remote-Logging bedeutet, dass ein Host Syslog-Meldungen auf einen anderen Host weiterversendet. Dieser andere Host schreibt die Meldungen dann in Dateien. Gewöhnlich konfiguriert man das so, daß die Meldungen nicht nur über das Netzwerk verschickt, sondern daneben auch lokal in Dateien geschrieben werden. Dies beugt Informationsverlust bei Netzwerkausfällen oder Störungen vor. Da Syslog eine wichtige Informationsquelle zur Analyse von Störungen ist, soll hier natürlich möglichst nichts fehlen.

Vorteile des Remote-Logging

Oft hat man in LANs einen zentralen Host, der Netzwerk-Syslog-Meldungen erhält, und diese in Dateien schreibt. Diesen Host nennt man Loghost.

Diese Konfiguration hat etliche Vorteile: So kommen die Meldungen zentral auf einer Maschine an, so dass man auch komplexere Störungen analysieren kann (wenn diese mehrere Server betreffen, zum Beispiel einen Mailserverausfall, weil DNS ausgefallen ist).

Ein weiterer Vorteil liegt im Fall von erfolgreichen Angriffen vor. Wenn ein Angreifer ein System kompromitiert hat, wird er in den meisten Fällen die Syslogdateien löschen oder manipulieren, um sich zu tarnen und seine Herkunft zu verschleiern. Wenn nun aber der Administrator einen Loghost verwendet, ist es unwahrscheinlicher, dass auch dieser gleichzeitig gehackt wird. So kann er auf dem Loghost die Meldungen analysieren und wichtige Informationen über den Angriff erlangen.

Ein dritter Vorteil der Zentralisierung ist die Vereinfachung automatischer Behandlung von Logfiles, zum Beispiel wird das Aufbereiten/Filtern und als Mail Verschicken erleichtert: Man muß diesen Vorgang nur auf einer Maschine pflegen.

Nachteile des Remote-Logging

Remote-Logging hat aber auch Nachteile, gerade, wenn man Syslog einsetzt. Syslog verwendet ausschließlich das UDP Protokoll. UDP Pakete werden direkt verschickt, ihr Empfang wird nicht bestätigt. Auf stark ausgelasteten Netzen kann es daher vorkommen, dass Meldungen verloren gehen, ohne dass man es bemerkt. Weiterhin kann ein Angreifer den Loghost überfluten, in dem er sehr viele sinnlose Meldungen verschickt. Dies kann die Last auf dem Loghost stark erhöhen, und im Extremfall dazu führen, dass er nur noch einen Teil der wichtigen Meldungen erhält, und die Netzwerklast kann zu weiteren Störungen führen. Bei sehr massiven Flut-Angriffen ist auch ein Vollaufen der Festplatte denkbar. In diesem Fall ist neben dem Verlust von Logmeldungen in der Regel mit weitereren empfindlichen Störungen zu rechnen, oft mit einem Totalausfall sämtlicher Dienste des Loghosts!

Ein Angreifer, der eine Maschine gehackt hat, und hier über root-Rechte verfügt, kann auch einen Netzwerk-Sniffer verwenden, um die Syslog-Nachrichten, die über das Netzwerk verschickt werden, mitzulesen. Er kann dadurch wichtige Informationen ausspionieren. Syslog gestattet leider keine Verschlüsselung oder andere Absicherung der Netzwerkkommunikation.

Konfiguration des Loghosts

Die Konfiguration des Loghosts ist einfach. Man muß lediglich den Empfang aktivieren, in dem man Syslog mit der Option -r startet. Bei SuSE-Systemen öffnet man dazu einfach die Datei /etc/rc.config, und ändert
/etc/rc.config

  SYSLOGD_PARAMS=""  

so, dass die Startoption -r verwendet wird. Die IP Adressen der Hosts, die den Loghost verwenden, sollte man in die Datei /etc/hosts eintragen, um Fehlern bei DNS Ausfällen vorzubeugen.

Kommt nun eine Nachricht über das Netzwerk, schaut Syslog anhand der Sender-IP Adresse nach, wie der Hostname des Systems lautet. Ist dieser beispielsweise de www.selflinux.de, so wird dieser Name im Logfile eingetragen. Dies ist unübersichtlich, und man möchte vermutlich die Ausgaben von ".selflinux.de unterdrücken (sofern der Teil davor eindeutig ist). Dazu verwendet man am einfachsten die Option -s, die die Domainanteile abschneidet. In unserem Beispiel würde der Administrator also -r -s selflinux.de verwenden. Hat er ein SuSE-System, trägt er einfach in /etc/rc.config ein:
/etc/rc.config

 SYSLOGD_PARAMS="-r -s selflinux.de"  

Syslog verwendet den UDP Port syslog. Dieser wird in der Datei /etc/services nachgesehen. Normalerweise soll Syslog die Portnummer 514 verwenden. Demzufolge muß folgende Zeile in der Datei /etc/services vorhanden sein:
/etc/services

 syslog          514/udp  

Dies ist bei gängigen Distributionen (SuSE, RedHat) jedoch bereits richtig eingetragen.

Nun muß Syslog neu gestartet werden, damit die Änderungen aktiv werden. Dazu schreibt man beispielsweise:
root@linux ~# /etc/rc.d/syslog restart

Auf SuSE Systemen kann man auch schreiben:
root@linux ~# rcsyslog restart

Nun akzeptiert der Syslog Nachrichten vom Netzwerk.

Konfiguration der anderen Hosts

Die Maschinen, die nun den Loghost verwenden sollen, müssen hierzu angepaßt werden. Ein neuer Eintrag in der Datei /etc/syslog.conf ist auf jedem Server notwendig. Möchte man alle Nachrichten auf den Loghost 192.168.1.1 loggen, verwendet man:
/etc/syslog.conf

  *.*                   @192.168.1.1  

Um nur wichtige Meldungen zu verschicken, kann man
/etc/syslog.conf

  *.warn                @192.168.1.1  

verwenden. Es kann auch mehrere solcher Zeilen geben, so kann man sich auch eine Konfiguration mit zwei Loghosts vorstellen. Über den Sinn solchen Vorgehens kann man natürlich streiten.

Nach dem Ändern dieser Datei muß Syslog neu geladen oder neu gestartet werden. Dazu kann man Syslog ein Hangup-Signal senden (SIGHUP), in dem man beispielsweise schreibt:
root@linux ~# killall -HUP syslog

oder auf SuSE-Systemen das Startscript verwenden:
root@linux ~# rcsyslog reload

Man kann Syslog aber auch einfach neu starten (stop/start). Allerdings gehen hier möglicherweise für einige Sekunden Meldungen verloren.

Beispiel für Logeinträge

Auf dem Loghost kann man dann gut das Netzwerksystem beobachten. Ein fiktives Beispiel:
/var/log/messages (fiktives Beispiel)

      
Mar 10 13:30:30 atlas syslogd 1.3-3: restart.

Apr  1 13:02:01 ns1 named[124]: XX+/127.0.0.1/1.1.168.192.
                                in-addr.arpa/PTR/IN
Apr  1 13:02:01 www httpd[123]: GET /login.cgi?username=steffen
Apr  1 13:02:03 www httpd[123]: Starting authorization for
                                "steffen" from "ws1.selflinux.de"
Apr  1 13:02:04 radius radiusd[125]: autorization request from
                                "www.selflinux.de" for "steffen"
Apr  1 13:02:04 db kernel: end_request: I/O error, dev 03:02 (hda),
                                sector 58138452
Apr  1 13:02:04 db postmaster[111]: Database error: disk read failed
                                (I/O error)
Apr  1 13:02:04 radius radiusd[125]: authorization request for
                                "steffen" failed (database error)
Apr  1 13:02:03 www httpd[123]: Authorization for "steffen" failed
                                (incorrect password)
      

In diesem fiktiven Szenario sieht man eine fehlgeschlagene Web-Anmeldung. Der Webserver (httpd) löste die IP Adresse auf (über "named" auf "ns1") und fragte dann bei einem Radius-Dienst auf einem separten Server nach. Dieser wiederum verwendete eine PostgreSQL Datenbank auf einem anderen Server. Diese hat ein großes Problem: Eine kaputte Festplatte ( sector 58138452 konnte nicht gelesen werden). Demzufolge kann PostgreSQL (postmaster) die Anfrage nicht bestätigen. Radius meldet also einen Fehler, den der Webserver als incorrect password fehlinterpretiert. Nicht das falsche Passwort war das Problem, sondern eine kaputte Festplatte! In diesem Fall würde man im Webserverlog sehr viele incorrect password finden, und einen Angriff vermuten. Doch durch die Verwendung eines Loghosts sind die Meldungen aller Komponenten zentral verfügbar. Hier hat das die Fehlersuche erheblich beschleunigt (der Administrator hat die Festplatte sofort gewechselt und die Bandsicherung zurückgespielt).

Konfigurationsvorschläge
""""""""""""""""""""""""

Serverkonfiguration

Bei der Installation eines Servers kann man überlegen, wie man mit großen Logfiles umgehen möchte. Hier ist zunächst von Interesse, dass große Logdateien Filesysteme füllen können. Hat man die Logdateien (in der Regel also das Verzeichnis /var/log) im selben Filesystem gemoutet wie beispielsweise das Root-Filesystem /, so ist damit zu rechnen, dass nach einer Logflut das System vollkommen unbenutzbar ist, also mit dem Ausfall sämtlicher Dienste.

Diese Gefahr kann man durch den Einsatz von Werkzeugen wie logrotate oder dem SuSE-Linux /etc/logfiles Mechanismus senken. Auf SuSE-Systemen trägt man hierzu jede Logdatei in /etc/logfiles ein. Hinter den Dateinamen schreibt man die Größe (z.B. +1024k) und den Zugriffsmodus (beispielsweise 640) und den Eigentümer (beispielsweise root.root). Die erste Option wird für das Dienstkommando find als Parameter verwendet, der zweite für chmod und der dritte für chown. Die manpages dieser drei Werkzeuge geben Auskunft über Art der verwendbaren Werte. Auf SuSE-Systemen sind in dieser Datei die voreingestellten Logdateien bereits eingetragen. Eigene Dateien muß man hier natürlich hinzufügen.

Eine weitere Möglichkeit wäre der Einsatz von separaten Filesystemen. Man kann z.B. /var auf eine andere Partition oder ein anderes LVM LV (logical volume) mounten. Dies hat jedoch auch Nachteile: es wird nicht der gesamte verfügbare Platz für Logfiles verwendet (demzufolge fällt das Logging früher aus), und insbesondere Angriffe und Störungen sind damit nicht mehr rekonstruierbar. Daher ist eine regelmäßige Überprüfung der freien Plattenkapazität durchzuführen, vorzugsweise automatisch, dann kann man es nicht vergessen.

Bootkonfiguration

Syslog sollte stets laufen. Syslog benötigt meist Schreibzugriff auf Festplatten. Bei Konfigurationen mit einem zentralen Loghost wird weiterhin das Netzwerk benötigt. Daher sollte man Syslog unmittelbar nach dem Hochfahren des Netzwerkes starten. Auf SuSE-Systemen verhält sich das bereits so.

Es ist denkbar, den Start von Syslog vor dem des Netzwerkes durchzuführen, wenn man keinen Loghost verwendet. Eventuell erhält man so mehr Meldungen.

Nach dem Start von Syslog sollte man den Kernel-Logger klogd starten. Sicherheitshalber wartet man z.B. eine Sekunde dazwischen. Die GNU/Linux-Startscripte sollten dies bereits so machen (bei SuSE-Systemen ist es der Fall).

Syslog-Konfiguration

Man sollte darauf achten, dass keine Meldungen überhaupt nicht geloggt werden. Meistens möchte man verschiedene Dateien haben, um schnell Meldungen zu finden. Oft werden mindestens die Dateien /var/log/messages und /var/log/warn verwendet. Letzere enthält nur wichtige Meldungen. Sehr üblich ist auch eine Datei /var/log/mail und eventuell /var/log/news für Meldungen des Mail- bzw. Newssystems. Häufig sieht man auch /var/log/allmessages oder /var/log/allinone, die sämtliche Meldungen enthalten.

Zusätzlich empfiehlt es sich, Meldungen auf einer virtuellen Konsole zu haben.

Weitere Informationen und ein Beispiel finden sich im Abschnitt "Die Konfigurationsdatei", siehe oben.

Einheitliche Netzwerkzeit

Bei der Analyse von Störungen ist es wichtig, Reihenfolgen und Abstände von Fehlermeldungen oder Fehlermails richtig feststellen zu können. Oft sind Zeitstempel bekannt. Diese sind natürlich Rechner-übergreifend nur verwendbar, wenn auch alle Maschinen die gleiche Vorstellung der Netzwerkzeit haben, deren Uhren also genau gleich bzw. synchron sind. Es empfiehlt sich also (insbesondere, wenn man keinen Loghost verwendet), die Netzwerkzeiten zu synchronisieren. Dazu verwendet man üblicherweise einen NTP (Network Time Protocol) Dienst, beispielsweise xntpd.

Firewall-Konfiguration

Firewalls sollten verhindern, dass UDP/514 Pakete vom Internet in das LAN geroutet werden, um Flut-Angriffen zu entgehen. Selbst wenn man keinen Loghost verwendet, könnte möglicherweise ein interner Host den Empfang von Netzwerk-Meldungen aktiviert haben. Auskunft darüber gibt das Kommando:
root@linux ~# netstat -an --inet | grep 514

Sicherheitshalber sollten Firewalls ohnehin alle nicht benötigten und nicht verwendeten Ports sperren.

Interne Firewalls müssen diese Pakete natürlich zwischen Loghost und den Logsystemen erlauben, aber sollten so eingestellt werden, daß nur die betreffenden IP-Adressen erlaubt sind.

Es ist zu beachten, dass die Absender-Adresse von UDP Paketen sehr einfach fälschbar ist. Demzufolge kann man bei Firewalls diese Adressen nicht zum Filtern verwenden. Man sollte hier an Interfaces blocken. Hat eine Firewall beispielsweise ein externes Interface eth1, sollte eine entsprechende Firewall-Regel den Empfang und Versand von Syslog-Paketen über dieses Interface unterbinden. Wenn die Firewall über eth0 an das interne Netz angebunden ist, kann hier dennoch ein Loghost verwendet werden, der die Firewallmeldungen empfängt.

Bei der Verwendung von Firewalls und Loghosts ist zu beachten, daß normalerweise unerlaubte Pakete geloggt werden, was zu einen hohen Aufkommen an Meldungen führt. Hier sollte Syslog so konfiguriert werden, dass diese Meldungen nicht über das Netzwerk geschrieben werden, wenn sich hier Probleme ergeben (Portscans könnten beispielsweise zu Flut-Verhalten führen).

Häufig sind aber die Außenanbindungen vergleichsweise langsam (beispielsweise E1 (2MBit) extern und 100Mbit intern), so daß hier interne Netzwerküberlastungen eher unwahrscheinlich sind.

Starten und Stoppen von Syslog
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Der Start erfolgt fast immer und ausschließlich automatisch beim Hochfahren des Systems über ein rc-Script, häufig /etc/rc.d/syslog. Bei gängigen GNU/Linux Distributionen ist das bereits so konfiguriert. Dieses Script startet oft (zum Beispiel bei SuSE-Systemen) auch automatisch den Kernel-Logger. Andere Systeme haben hier eventuell ein eigenes rc-Script. Dieses sollte dann in jedem Fall direkt nach Syslog gestartet werden.

Um Syslog manuell zu starten, verwendet man:
root@linux ~# /etc/rc.d/syslog start

oder bei SuSE-Systemen kurz:
root@linux ~# rcsyslog start

Um den Dienst zu stoppen, schreibt man analog dazu stop anstatt start. 

Meldungen selbst erzeugen
^^^^^^^^^^^^^^^^^^^^^^^^^

Es gibt mehrere Fälle, in denen man selbst Meldungen erzeugen möchte. Denkbar sind beispielsweise Cron-Jobs, die neben dem Verschicken der EMail mit den Script-Ausgaben kurze Erfolgs- oder Mißerfolgsmeldung über Syslog schreiben sollen. Eine weitere Anwendung sind automatisch ausgeführte Scripte, beispielsweise /etc/ppp/ip-up oder rc-Scripte.

Es gibt ein kleines, einfaches Werkzeug, mit dem man Syslog-Meldungen erzeugen kann. Es heißt logger. Diesem Werkzeug kann man über Optionen sagen, wie eine Meldung zu loggen ist. Hier kann man beispielsweise Priority und Facility angeben.

Die wichtigsten Optionen sind:
Option 	Beschreibung
-i 	Prozeß-ID mit in die Nachricht schreiben
-p <Fac.>.<Pri.> 	Angegenene Priorität und Facility
verwenden. Mögliche Werte sind die
gleichen wie die in /etc/syslog.conf
verwendbaren, siehe Abschnitt "Quellen von
Meldungen" und "Priorität von Meldungen".
Wird diese Option nicht angegeben, wird
user.notice verwendet.
-t <Tag> 	Angegebenes "Tag" verwenden. Meist wird
hier der Name des Scriptes verwendet.

Es gibt zwei Arten, logger aufzurufen. Einmal kann man die Meldungen mit in die Kommandozeile schreiben, direkt hinter die Optionen. Mindestens wenn die Meldung Minuszeichen (-) enthalten könnte, sollte man sie mit -- abtrennen, um zu vermeiden, dass Meldungsteile als Optionen mißinterpretiert werden. Ein Beispiel:
user@linux ~$ logger -i -t MeinProgramm -- Ich bin eine Meldung.

Dies erzeugt dann folgenden Logdatei-Eintrag:
user@linux ~$ logger -i -t MeinProgramm -- Ich bin eine Meldung.
Apr 13 13:02:04 atlas MeinProgramm[6178]: Ich bin eine Meldung.

Die zweite Art des Aufrufes unterscheidet sich in der Art, wie die Meldung übergeben wird. Wird nämlich keine auf der Kommandozeile angeben, liest logger die Standard-Eingabe. Damit kann man also schreiben:
user@linux ~$ echo "Ich bin eine Meldung." | logger -i -t MeinProgramm

was zum gleichen Ergebnis führt. logger geht auch mit mehreren Eingabezeilen korrekt um (jede Zeile wird eine Meldung). In Scripten kann man deshalb beispielsweise komfortabel schreiben:
Beispielscript

    
LOGGER="/usr/bin/logger -t `basename $0`[$$]"

{
  test -e /etc/irgenteinedatei || echo "Error, Datei nicht gefunden!";
  date;
  echo "eine andere Fehlermeldung";
} | $LOGGER
    
    

Dies erzeugt auf elegante Art und Weise folgende Einträge:
/var/log/messages

 
Apr 13 13:20:45 atlas script.sh[6338]: Error, Datei nicht gefunden!
Apr 13 13:20:45 atlas script.sh[6338]: Sat Apr 13 13:20:45 MEST 2002
Apr 13 13:20:45 atlas script.sh[6338]: eine andere Fehlermeldung
     
Logdateien auswerten
^^^^^^^^^^^^^^^^^^^^

Sehr wichtig ist es natürlich, die erzeugten Logdateien auszuwerten. Gewöhnlich hat ein Administrator jedoch nicht die Zeit, täglich tausende von Zeilen zu lesen.

Hier benutzt man Werkzeuge, um diese Arbeit zu automatisieren. Ein Beispiel hierfür ist LogWatch, das von RedHat verwendet wird. Ein weiteres ist logmail, das man sich von en http://sws.dett.de/logmail herrunterladen kann (es enthält auch eine deutsche Dokumentation). Sicherlich gibt es viele weitere ähnliche und leistungsfähigere Werkzeuge.

Verwendet man logmail, so kann man sich eine Konfigurationsdatei anlegen, in dem spezifiziert wird, welche Einträge per EMail an wen verschickt werden. Bei Loghosts kann man zum Beispiel alle Meldungen von postmaster an den PostgreSQL Administrator und alle Meldungen von named an den DNS-Administrator verschicken, und alle Meldungen an einen dritten Administrator. Des weiteren sollte konfiguriert werden, welche Meldungen man nicht haben möchte, sonst erhalten die Administratoren viele langweilige Meldungen. Logmail gestattet es auch, Loghost-Meldungen anhand des Hostnamens per EMail zu verteilen. So kann man den entsprechenden Systemadministratoren ihre Meldungen zustellen.

Man kann sich alle Meldungen, die man per EMail erhält, aber nicht mehr erhalten möchte, einfach in die Konfigurationsdatei als Filter mit aufnehmen. Dann werden diese in Zukunft nicht mehr verschickt. Derartige Systeme bedürfen natürlich der Pflege. In wenigen Tagen erhält man dann aber nur noch neue, interessante Meldungen per EMail, und kann nicht vergessen, Logfiles auszuwerten. So erkennt man auch Störungen, die nachts auftraten, oder bekommt Mitteilungen über Angriffsversuche. Zusätzlich senkt das die Gefahr, dass ein Angreifer Logfiles manipulieren kann, da unter Umständen bereits eine EMail verschickt wurde, die der Angreifer nicht mehr aufhalten oder ungeschehen machen kann.

Nur durch derartiges Vorgehen ist es möglich, mehrere Server richtig zu betreuen, da im Produktionsbetrieb natürlich keine Zeit bleibt, täglich stundenlang Logfiles auszuwerten, die fast immer langweilig sind. 

Andere Systemlogger
^^^^^^^^^^^^^^^^^^^

Neben Syslog gibt es weitere Systemlogger, die man verwenden kann. Beispiele sind socklock und syslog-ng. Beide haben interessante Funktionen und Vorteile gegenüber Syslog.

8 Weitere Informationsquellen

Es folgt eine unvollständige Liste:

* syslog manpage

* klogd manpage

HOWTOs oder andere Dokumentationen sind mir nicht bekannt. Über Hinweise freue ich mich natürlich jederzeit. 

Autor

    Steffen Dettmer steffen@dett.de
	
Formatierung

    Matthias Hagedorn matthias.hagedorn@selflinux.org


syslog-ng
---------

Einführung
^^^^^^^^^^

Logfiles werden gerne übersehen und sind doch so extrem wichtig und hilfreich. Einer der großen Vorzüge von Linux ist, dass es fast alles im System protokollieren kann. So kann man immer nachvollziehen, was wann wo passiert oder eben schief läuft.

Logging wird gerade aus Sicherheitsgründen immer wichtiger. Hierüber lässt sich früh erkennen, wann wo im System ein Einbruchsversuch läuft. Neben dem Logging sind vor allem gute Logfile-Analyse-Werkzeuge notwendig.

Der Standard  syslog Daemon unter Linux und vielen Unix Varianten ist in vielerlei Hinsicht sehr eingeschränkt. Er konnte mir nicht das liefern, was ich brauchte, um Logfiles vernünftig und einfach auszuwerten.

Ich bin nicht der einzige, dem der  syslog Daemon ziemlich angestaubt und unzulänglich vorkam. Und so machte sich dann Balazs Scheidler 1998 auf den Weg, einen neuen besseren  syslog zu schreiben. Diesen nannte er syslog-ng. Das ng steht für New Generation.

syslog-ng ist mittlerweile ein fest etablierter und stabil laufender Ersatz für den  syslog. Alle großen Distributionen haben ihn mit an Board, jedoch muss er meist noch aktiviert werden. Es spricht eigentlich alles dafür, auf syslog-ng umzusteigen, sobald man Möglichkeiten braucht, die über den normalen  syslog hinausreichen.

Diese Beschreibung bezieht sich auf die syslog-ng Version 1.5.15, wie sie in de Debian Woody enthalten ist. 

Ein Anwendungsfall
^^^^^^^^^^^^^^^^^^

Als ich anfing, mir ein Logfile-Analyse-Werkzeug zu bauen, merkte ich, dass der Standard  syslog Dämon von Linux und vielen Unix-Systemen doch erhebliche Einschränkungen mit sich bringt. Ich wollte ein Logfile in einer ähnlichen Form, wie es moderne Anwendungen wie cups, samba oder Apache machen. Das Datum sollte mit Jahresangabe sein, Priorität, Facility und das Programm, was loggt, sollten ebenfalls auftauchen. Eine Zeile sollte etwa so aussehen:

[2003/10/13 12:00:42] info mail fetchmail fetchmail[30924]: No mail for ...
    

Zuerst das Datum in einer samba-like Form, dann die Log-Priorität, die Facility (also der Systembereich, der loggt), das Programm und die Message. Alle erzeugten Logs des Systems, die über  syslog laufen, sollten in einer Logdatei landen. Hierzu fügte ich in der syslog-ng.conf folgende Zeilen hinzu:

destination mysyslog {
  file("/var/log/mysyslog"
  owner("root")
  group("adm")
  perm(0640)
  template( "[$YEAR/$MONTH/$DAY $HOUR:$MIN:$SEC] $PRIORITY
             $FACILITY $PROGRAM $MESSAGE\n")); };

log { source(src);  destination(mysyslog); };
    

Mit destination wird ein  destination-Objekt festgelegt, welches mysyslog heißt und mit der Datei /var/log/mysyslog verbunden wird. Das Ausgabe-Format beschreibt ein Template. Über die Zeile log wird dann ein zuvor definiertes  source-Objekt und mein  destination-Objekt zu einer Log-Aktion zusammengefügt. 

Konfigurationsdetails
^^^^^^^^^^^^^^^^^^^^^

Die Konfigurations-Syntax ist an C/Java/PHP angelehnt und sehr einfach gehalten. Man sollte jedoch aufpassen, Semikolons immer richtig zu setzen.

Leider gehört es zum Charakter von Linux, dass fast jeder Autor eines Paketes sich in einer eigenwilligen Konfigurations-Syntax verewigt. Wie schön wäre es, wenn gerade bei der Konfigurations-Sprache sich ein Standard durchsetzt, an den sich alle halten. Manche großen Pakete wie en samba, en cups,  apache machen schon gute Vorgaben, die andere Paketentwickler übernehmen. Der Einstieg in neue Pakete wird so wesentlich vereinfacht, weil man nur noch wissen muss, was man konfigurieren will und nicht mehr, wie man überhaupt konfigurieren kann. Ein vielversprechender Ansatz ist en http://config4gnu.sourceforge.net/docs/article.html.

Die gesamte Konfiguration erfolgt über die Datei syslog-ng.conf, die bei Debian Linux unter /etc/syslog-ng/syslog-ng.conf liegt.

In dieser Konfigurations-Datei gibt es prinzipiell folgende Einträge:

options { option; option; .. };
    

Hiermit können globale Optionen festgelegt werden.

source <identifier> { source-driver(params); ... };
    

Mit jeder source Zeile wird eine syslog-Quelle festgelegt.

filter <identifier> { expression; ... };
    

Hiermit können beliebig viele  Filter-Objekte angelegt werden, die später in der log-Zeile benutzt werden können. Expression beschreibt die Filterregel.

destination <identifier> {dest-driver(params); ... };
    

Hiermit kann man Ziel-Objekte anlegen, wohin also geloggt werden soll und mit welchen Einstellungen.

log { source(s1); source(s2); ...; filter(f1); filter(f2); ...; destination (d1);
      destination(d2); ...; flags( flag1; ...) }
    

Hiermit werden  log-Objekte angelegt und  log-Objekte sind die einzigen, die auch Aktionen ausführen, wenn sie definiert sind. In dem  Log-Objekt werden zuvor definierte Objekte ( source,  filter,  destination) zusammengefügt und das Logging somit aktiviert.

Nochmal zum Verständnis: Man kann beliebig viele source, filter und destination-Objekte anlegen, die von sich aus gar nichts tun. Erst das Einbinden in ein log-Objekt, aktiviert eine Logausgabe. Das log-Objekt ist das einzige, was irgendwie aktiv wird.

Dieser objektorientierte Aufbau ist sehr leicht überschaubar und wartbar. Nach ein wenig Praxis wird man spüren, wie angenehm diese Form zu handeln ist.

Das options Objekt
""""""""""""""""""

Hier kann man globale Einstellungen vornehmen. Das meiste ist für erste Gehversuche schon korrekt eingestellt, es geht hauptsächlich um Feintuning. Einige wichtige Einstellungen sind:

    sync(n-lines)
    Hiermit kann man festlegen, wieviele Log-Zeilen auflaufen sollen, bis gesynct wird, bis also die Daten tatsächlich auf der Platte abgelegt werden und nicht noch in irgendeinem Puffer im Ram stecken. Wer sicher gehen will und auch keine Performance-Probleme hat, sollte hier 0 einstellen - es wird dann sofort jede Zeile geschrieben.
    log_fifo_size(n-lines)
    Anzahl der Zeilen, die zwischengepuffert werden können. Ist wichtig, wenn mehr Logs einlaufen, als syslog-ng wegschreiben kann. Das kann z. B. passieren, wenn eine kurzzeitige Logzeilen-Flut hereinbricht. Ein typischer Wert ist 1000.


Das source Objekt
"""""""""""""""""

Hier kann man die Quellen angeben, woher syslog-ng Meldungen empfangen soll. Unter debian Linux reicht für das normale Logging folgende Zeile:

source src { unix-stream("/dev/log"); internal(); };
     

Es wird hier ein neues  Source-Objekt namens src angelegt. Dieses Source-Objekt wird nun mit zwei Quellen verbunden, zum einen mit /dev/log, zum anderen mit internal. Der Typ internal steht für die Messages, die syslog-ng selber erzeugt. Die sollte man also immer mit aufnehmen. /dev/log ist vom Typ unix-stream, ein Stream mit unix-style Übertragung, was man typischerweise unter Linux nutzt. Ein ähnlicher Typ ist unix-dgram, der in BSD Systemen verwendet wird. Unter Linux funktioniert generell auch unix-dgram, es ist aber nicht so sicher, weil bei zu hoher Systemlast Logs verloren gehen können.

Weitere Möglichkeiten von Typen einer Datenquelle sind file, pipe, udp, tcp. Hiermit kann man verrückte Sachen anstellen. Im Normalfall dürfte lediglich udp/tcp noch interessant sein, mit dem syslog-ng als Loghost übers Netz fungieren kann. Er verhält sich dann genauso, wie ein  syslog, der per  udp lauscht. Eine vollständige  udp Quelle sieht so aus:

udp( ip(0.0.0.0) port(514) )
     

Wobei ip und port auch weggelassen werden können, wenn die hier angegebenen Defaultparameter genommen werden sollen. Ip steht für die IP-Adresse, auf der gelauscht werden soll. Hat ein Rechner also mehrere IP-Adressen, kann man hier Einschränkungen machen. Default ist 0.0.0.0, was auf allen Adressen lauscht. Man kann nicht angeben, das z. B. nur Logs von einem bestimmten Host angenommen werden. Port gibt den Port an, auf dem gelauscht werden soll. Default ist lt. altem  syslog der Port 514.

 UDP ist ein verbindungsloses Protokoll, es kann also passieren, dass bei verlorenen Paketen auch Logs verloren gehen, weil diese nicht erneut angefordert werden. Besser ist es daher,  tcp einzusetzen. Konfiguriert wird es genauso, man kann auch den gleichen Port nutzen, insofern man auf das sonst dort sitzende rshell verzichten kann (seit ssh sollte die eh überflüssig sein). Bei  tcp gibt es zusätzlich noch den Parameter max-connections(n), der die Anzahl der Verbindungen limitiert.

Wer allerdings von alten  syslogs aus Logs entgegennehmen muss, der ist auf  udp angewiesen, weil die nur  udp können.

Es gibt noch eine weitere Quelle, die man evtl. nicht vergessen darf - die  Kernel-Messages. Wenn man auf seinem Linux-System einen klogd laufen hat, dann holt dieser sich alle Kernelmessages aus /proc/kmsg und reicht sie an den Syslog- Daemon (hier: syslog-ng) weiter. Somit hat man diese Messages automatisch. Das dürfte bei den meisten Distributionen der Standard-Fall sein. Der klogd hat auch den Vorteil, das er manche Logs noch weiter aufbereitet, z. B. Funktionsnamen über die .map Kernel Files dekodiert. Dadurch können Probleme leichter gefunden werden.

Läuft kein klogd, so muss man diese Kernel-Messages selber abholen. Eine komplette Sourcezeile könnte dann so aussehen:

source src { unix-stream("/dev/log"); internal(); file("/proc/kmsg");};
     

Manche Distributionen benutzen hier auch pipe() statt file() für /proc/kmsg.

Das destination Objekt
""""""""""""""""""""""

Mit dem Destination Objekt kann man Ziele festlegen, wohin ein Log-Stream gehen soll. Normal ist dies eine Datei. Ein einfaches Ziel könnte so aussehen:

destination syslog { file("/var/log/syslog" owner("root") group("adm") perm (0640)); };
     

Hier wird das Ziel mit dem Namen syslog angelegt, wobei es mit der Datei /var/log/syslog verbunden wird. Diese Datei soll mit dem Benutzer root.adm angelegt werden und 0640er Rechte haben.

Neben den file()-Optionen owner(), group(), perm() gibt es weitere, die man bei speziellen Ansprüchen nutzen kann. Eine sehr interessante Option ist die Möglichkeit, ein Ausgabeformat mit template() festzulegen. Dies hatte ich weiter oben an einem Beispiel schon verdeutlicht. Hiermit kann man sich nahezu beliebige Logfile-Formate erstellen. Das Template ist ein String, in dem Makros mit einem $MakroName eingefügt werden können.

Eine typisches Template könnte so aussehen:

template( "[$YEAR/$MONTH/$DAY $HOUR:$MIN:$SEC] $PRIORITY $FACILITY $MESSAGE\n")
     

Hier wird zuerst das Datum in einer typischen Form zusammengefügt, dann die Priorität, die Facility und die Message ausgegeben.

Folgende Makros sind verfügbar:
FACILITY 	Facility der Message. Das ist eine der vordefinierten Gruppen des Subsystems, von der eine Message kommt. Möglich sind hier auth, auth-priv, cron, daemon, ftp, kern, lpr, mail, mark, news, syslog, user, uucp, local0 - local7. Was welche bedeuten und welches Programm welche Facility benutzt, ist nicht immer einfach herauszufinden. Ein Beobachten der syslogs hilft am besten. Viele Programme können vor dem Kompilieren eingestellt werden, unter welcher Facility sie loggen. Das Facility-System kann man als starr und veraltet betrachten, weil es für die meisten Filteraufgaben zu grob und unflexibel ist.
PRIORITY oder LEVEL 	Die Priorität der Message. Hier gibt es: debug, info, notice, warn, err, crit, alert, emerg.
TAG 	Die Priorität und Facility als 2-Zeichen Hexzahl codiert.
DATE, FULLDATE, ISODATE 	Das Datum in verschiedenen standardisierten Formaten.
YEAR 	Das Jahr 4-stellig.
MONTH 	Der Monat 2-stellig numerisch, ggf. führende 0.
DAY 	Der Tag 2-stellig numerisch, ggf. führende 0.
WEEKDAY 	Wochentag 3 Buchstaben, wie unter Unix gewohnt. (Mon, Tue...)
HOUR 	Die Stunde 2-stellig, ggf. führende 0.
MIN 	Die Minute 2-stellig, ggf. führende 0.
SEC 	Die Sekunden 2-stellig, ggf. führende 0.
FULLHOST 	Vollständig qualifizierter Host, also host.domain
HOST 	Hostname ohne Domainzusatz.
PROGRAM 	Das Programm, welches die Messages abgesetzt hat. Hierüber lässt sich oft flexibler filtern, als über die Facilitys.
MSG oder MESSAGE 	Die eigentliche Message. Da es im alten  syslog keine Möglichkeit gab, das Programm oder den Prozess mitzuloggen, hat es sich als Standard herausgebildet, in die Message als erstes das Programm mit Prozessnummer anzugeben (Programm[ps-id]: ), welches die Message ausgab. Bei Unterprozessen eines Programmes, wird normal Programm/Unterprozess[ps-id]: verwendet. Erst hinter dem Doppelpunkt beginnt die eigentliche Message. Verlassen kann man sich jedoch auf diese Regel nicht, Ausnahmen gibt es immer wieder.

Es gibt bei file() einen leistungsfähigen Mechanismus, die Makro-Substitution für den Dateinamen. So kann man sich dynamische Logfilenamen generieren. Hier kann man die gleichen Makros wie für template() benutzen. Ein

destination mylog { file("/var/log/syslog-$HOST" owner("root") group("adm") perm(0640)); };
     

loggt z. B. jeden Host in in eine getrennte Datei in der Form syslog-MeinErsterHost, syslog-NochEinHost. Natürlich muss man hier auch vorsichtig sein, um DoS- Attacken nicht Tür und Tor zu öffnen. Wer jedoch spezielle Möglichkeiten des Loggens braucht, kommt mit diesem Feature vielleicht weiter.

Neben file gibt es noch folgende Ziel-Typen bzw. Ziel-Treiber: tcp, udp, unix- stream, unix-dgram, fifo, usertty, program.

Udp oder tcp nutzt man ähnlich, wie schon bei dem Source-Objekt beschrieben. Als Ziel angegeben, schickt der syslog-ng nun diesen Log-Stream zu einem anderen Rechner per tcp oder udp. Ein Beispiel:

destination a_udp { udp( "192.168.0.12" port(514) ); };
     

Hier sollen an den udp-Port 514 des Rechners mit der IP 192.168.0.12 Messages verschickt werden. Port 514 ist sowieso Default, könnte hier also auch weggelassen werden.

Hier gilt auch: Udp nimmt man aus Kompatibilitätsgründen, weil sich dann syslog-ng wie ein altes  syslog verhält. Tcp ist das sicherere Verfahren, weil bei diesem verbindungsorientierten Protokoll Pakete nicht verloren gehen können.

Auch udp ist relativ sicher, es werden nicht regelmäßig Packete verloren gehen, vor allem nicht im lokalen Netzwerk. Nur wenn Hardware oder Leitungen defekt sind, kommt es zu Datenverlusten. Aber auch tcp kann durch einen kaputte Leitung nichts mehr schicken, es kann lediglich bei einer kurzen Störung erneut versenden. Man sollte sehen, dass udp Jahrzehnte erfolgreich für diese Aufgabe eingesetzt wird.

Mit usertty kann man Meldungen auf dem Terminal eines Benutzers ausgeben, der natürlich eingeloggt sein muss. Hier ein Beispiel:

destination admin_tty { usertty(admin); };
     

Sollen Meldungen an alle eingeloggten Benutzer gehen, nimmt man:

destination warn_to_all { usertty(*); };
     

Das filter Objekt
"""""""""""""""""

Filter Objekte legen fest, wie Meldungen von einem Source-Objekt gefiltert werden sollen. Hiermit lassen sich also gewünschte Messages aus dem gesamten Datenstrom eines Source-Objektes herauspicken. Und das ist gut, wenn man z.B. in einem Ziel nur bestimmte Meldungen loggen möchte. Auch der alte  syslog hat eine einfache Filtersprache, um Logausgaben in verschiedene Logdateien zu schreiben.

Mit syslog-ng kann man wesentlich erweitert und verfeinert filtern.

Ein typisches Filterobjekt könnte so definiert werden:

filter f_cnews { level(notice, err, crit) and facility(news); };
     

Hier werden alle Meldungen durchgelassen, die vom Level oder der Priority auf notice, err oder crit stehen und die die Facility news haben.

Filterfunktionen lassen sich mit and, or, not verknüpfen und auch klammern. Somit kann man sehr leistungsfähige Filterkonstrukte erstellen.

Wer nicht genau weiß, wie and, or, not aufgelöst werden, sollte besser einmal zuviel klammern, als sich später zu wundern, warum was merkwürdiges bei heraus kommt. Das hilft auch anderen, die die Konfiguration verstehen wollen und auch nicht so genau bescheid wissen. Kurz gesagt bindet and mehr als or mehr als not. Ganz ähnlich wie Punktrechnung vor Strichrechnung kommt. a or b and c ist was anderes wie (a or b) and c.

Folgende Filterfunktionen gibt es:

    facility(facility1,facility2,...)
    Lass alle Messages durch, die dieser Facility entsprechen.
    level(prio1, prio2,...) (oder synonym priority())
    Lass alle Messages durch, die der angegebenen Priorität/Level entsprechen.
    program(regexp)
    Alle Meldungen, die vom Programm kommen, worauf regexp passt, werden durchgelassen. Hierbei ist regexp ein  regulärer Ausdruck. Hat man Whitespaces im Programmnamen, sollte man den Ausdruck zwischen doppelte Anführungsstriche setzen.
    host(regexp)
    Filterung nach Host, woher die Message kommt, ebenfalls regulärer Ausdruck. Ein Tipp, wenn du nichts von regulären Ausdrücken verstehst: Nimm einfach den Hostnamen, z. B. so: host(myhost). Genau genommen müsstest du host("^myhost$") schreiben.
    match(regexp)
    Dies lässt nur Meldungen durch, wo die eigentliche Message auf dieses Muster passt. Dies ist ein sehr leistungsfähiger Mechanismus, lassen sich doch so ganz gezielt Meldungen herausfischen. Es gibt fast nichts, was man nicht mit regulären Ausdrücken erschlagen könnte.
    filter(filter_name)
    Um kompliziertere Filterregeln zu erstellen, kann man mehrere Filter zu einem neuen Filterkonstrukt zusammenfügen. Hiermit kann man also andere Filter in einem neuen Filter aufrufen. Damit kann man verschachtelte Filterkonstrukte erstellen. Es ist oft auch für die Lesbarkeit besser, zuerst mehrere Teilfilter zu definieren, die man dann in einer weiteren Filterregel zusammenfügt.

Mach keine Meisterschaft daraus, möglichst komplizierte Konfigurationen zu produzieren. Durch komplizierte Filterregeln kann man das hier durchaus schaffen. Das zeigt zwar deine intellektuellen Fähigkeiten, produziert aber schwer wartbare Konfig-Dateien. Mach es so einfach wie möglich, andere werden es dir danken.

Das log Objekt
""""""""""""""

Alle bisherigen Objekte waren Vorarbeiten, um jetzt Zeilen zu generieren, die wirklich Aktionen auslösen. Denn ohne die log-Objekte würde gar nichts passieren. Die anderen Objekte sind nur Daten-Definitionen. Die log-Objekte führen das eigentliche Logging aus, in dem sie die zuvor definierte Source-, Destination- und Filter-Objekte zu einer Log-Aktion verbinden. Eine typische Zeile sieht so aus:

log { source(src); filter(f_syslog); destination(syslog); };
     

Es wird hier also vom Source src gelesen, diese Messages durch den filter f_syslog geschickt und dann zum Ziel syslog geschrieben.

Mehrere Sourcen bindet man mit mehreren source()-Statements ein.

log { source(src); source(src1); filter(f_syslog); destination(syslog); };
     

Mehrere Filter und sogar mehrere Ziele lassen sich nach gleichem Schema einbinden.

Hinter destination() lässt sich auch noch flags() angeben, mit dem man erweitere Funktionalitäten festlegen kann. 

Beispieldatei
^^^^^^^^^^^^^

Hier ist ein Beispiel einer kompletten Konfig-Datei, wie ich sie unter de Debian Woody verwende. Die Kernel-Messages hole ich hier direkt ohne den klogd. Dieser darf also nicht gestartet sein. Es gibt unter de Debian Woody noch einige Probleme im Zusammenspiel klogd/syslog-ng.

#
# Syslog-ng configuration file, compatible with default Debian syslogd
# installation. Originally written by anonymous (I can't find his name)
# Revised, and rewrited by me (SZALAY Attila sasa@debian.org)

# First, set some global options.
options { long_hostnames(off); sync(0); stats(3600); };

#
# This is the default behavior of sysklogd package
# Logs may come from unix stream, but not from another machine.
#
source src { unix-stream("/dev/log"); internal(); file("/proc/kmsg"); };

#
# If you wish to get logs from remote machine you should uncomment
# this and comment the above source line.
#
# source src { unix-dgram("/dev/log"); internal(); udp(); };


# After that set destinations.

# First some standard logfile
#
destination authlog { file("/var/log/auth.log" owner("root")
                      group("adm") perm(0640)); };
destination syslog  { file("/var/log/syslog" owner("root")
                      group("adm") perm(0640)); };
destination cron    { file("/var/log/cron.log" owner("root")
                      group("adm") perm(0640)); };
destination daemon  { file("/var/log/daemon.log" owner("root")
                      group("adm") perm(0640)); };
destination kern    { file("/var/log/kern.log" owner("root")
                      group("adm") perm(0640)); };
destination lpr     { file("/var/log/lpr.log" owner("root")
                      group("adm") perm(0640)); };
destination mail    { file("/var/log/mail.log" owner("root")
                      group("adm") perm(0640)); };
destination user    { file("/var/log/user.log" owner("root")
                      group("adm") perm(0640)); };
destination uucp    { file("/var/log/uucp.log" owner("root")
                      group("adm") perm(0640)); };


# This files are the log come from the mail subsystem.
#
destination mailinfo { file("/var/log/mail.info" owner("root")
                       group("adm") perm(0640)); };
destination mailwarn { file("/var/log/mail.warn" owner("root")
                       group("adm") perm(0640)); };
destination mailerr  { file("/var/log/mail.err" owner("root")
                       group("adm") perm(0640)); };

# Logging for INN news system
#
destination newscrit   { file("/var/log/news/news.crit" owner("root")
                         group("adm") perm(0640)); };
destination newserr    { file("/var/log/news/news.err" owner("root")
                         group("adm") perm(0640)); };
destination newsnotice { file("/var/log/news/news.notice" owner("root")
                         group("adm") perm(0640)); };

# Some `catch-all' logfiles.
#
destination debug      { file("/var/log/debug" owner("root")
                         group("adm") perm(0640)); };
destination messages   { file("/var/log/messages" owner("root")
                         group("adm") perm(0640)); };


# The root's console.
#
destination console { usertty("root"); };

# Virtual console.
#
destination console_all { file("/dev/tty8"); };

# The named pipe /dev/xconsole is for the nsole' utility.  To use it,
# you must invoke nsole' with the -file' option:
#
#    # xconsole -file /dev/xconsole [...]
#
destination xconsole { pipe("/dev/xconsole"); };

destination ppp { file("/var/log/ppp.log" owner("root")
                  group("adm") perm(0640)); };

# Here's come the filter options. With this rules, we can set which
# message go where.

filter f_authpriv  { facility(auth, authpriv); };
filter f_syslog    { not facility(auth, authpriv); };
filter f_cron      { facility(cron); };
filter f_daemon    { facility(daemon); };
filter f_kern      { facility(kern); };
filter f_lpr       { facility(lpr); };
filter f_mail      { facility(mail); };
filter f_user      { facility(user); };
filter f_uucp      { facility(uucp); };

filter f_news      { facility(news); };

filter f_debug     { level(debug) and not
                     facility(auth, authpriv, mail, news); };
filter f_messages  { level(info .. warn) and not
                     facility(auth, authpriv, cron, daemon, mail, news); };
filter f_emergency { level(emerg); };

filter f_info      { level(info); };
filter f_notice    { level(notice); };
filter f_warn      { level(warn); };
filter f_crit      { level(crit); };
filter f_err       { level(err); };

filter f_cnews     { level(notice, err, crit) and facility(news); };
filter f_cother    { level(debug, info, notice, warn) or
                     facility(daemon, mail); };

filter ppp         { facility(local2); };


log { source(src); filter(f_authpriv); destination(authlog); };
log { source(src); filter(f_syslog); destination(syslog); };
#log { source(src); filter(f_cron); destination(cron); };
log { source(src); filter(f_daemon); destination(daemon); };
log { source(src); filter(f_kern); destination(kern); };
log { source(src); filter(f_lpr); destination(lpr); };
log { source(src); filter(f_mail); destination(mail); };
log { source(src); filter(f_user); destination(user); };
log { source(src); filter(f_uucp); destination(uucp); };
log { source(src); filter(f_mail); filter(f_info);
      destination(mailinfo); };
log { source(src); filter(f_mail); filter(f_warn);
      destination(mailwarn); };
log { source(src); filter(f_mail); filter(f_err);
      destination(mailerr); };
log { source(src); filter(f_news); filter(f_crit);
      destination(newscrit); };
log { source(src); filter(f_news); filter(f_err);
      destination(newserr); };
log { source(src); filter(f_news); filter(f_notice);
      destination(newsnotice); };
log { source(src); filter(f_debug); destination(debug); };
log { source(src); filter(f_messages); destination(messages); };
log { source(src); filter(f_emergency); destination(console); };

#log { source(src); filter(f_cnews); destination(console_all); };
#log { source(src); filter(f_cother); destination(console_all); };


log { source(src); filter(f_cnews); destination(xconsole); };
log { source(src); filter(f_cother); destination(xconsole); };

log { source(src); filter(ppp); destination(ppp); };
    
Probleme und Offenes
^^^^^^^^^^^^^^^^^^^^

Zusammenspiel klogd und syslog-ng
"""""""""""""""""""""""""""""""""

Es gibt ein paar Probleme im Zusammenspiel des klogd mit syslog-ng. Unter de Debian Woody reicht der klogd nur Meldungen korrekt weiter, wenn unter syslog-ng die Methode unix-dgram("/dev/log") anstatt unix-stream("/dev/log") im Source Objekt verwendet wird. Auch gibt es Probleme, wenn syslog-ng neu gestartet wird, ohne gleichzeitig klogd neu zu starten. Der Datenaustausch von klogd an syslog-ng funktioniert dann nicht mehr korrekt.

Einige Distributionen sind deshalb dazu übergegangen, entweder den klogd nicht zu verwenden oder aber im syslog-ng Init-skript sowohl syslog-ng wie auch klogd zu starten und zu stoppen. Dadurch werden beide Daemons immer korrekt im Tandem hoch- und runtergefahren. Evtl. sollte man 1 Sekunde nach Start des syslog-ng warten, bevor man klogd startet.

Loggt man ohne klogd, fehlt der Prefix kernel: im Logfile, den klogd normal hinzufügt. Modernere Versionen von syslog-ng haben hierfür einen zusätzlichen Befehl, um diesen Prefix selber zu generieren. In einer Source-Zeile könnte dann z. B. stehen:

 pipe("/proc/kmsg" log_prefix("kernel: ")).
     

Unter de Debian Woody sollte man also einfach ohne klogd arbeiten oder aber die Init-Skripte anpassen.

Referenzen
""""""""""

    en http://www.balabit.com/products/syslog_ng/
    Referenz Manual im Source-tar (1.5.15) unter doc/sgml/syslog-ng.txt
    de http://home.datacomm.ch/prutishauser/texte/syslog-ng-de.txt
    Beispiele im Source-tar (1.5.15) unter contrib/syslog-ng.conf.*
    Beispiele im Source-tar (1.5.15) unter doc/syslog-ng.conf.*
    de http://www.linux-magazin.de/Artikel/ausgabe/2003/11/tagebuch/tagebuch.html
    man syslog-ng
    man syslog (lohnt sich, weil viele Standards und Definitionen von  syslog auf syslog-ng übernommen wurden.)
    man grep (Beschreibung  reguläre Ausdrücke (regexp))

Autor

    Winfried Müller wm@reintechnisch.de
	
Formatierung

    Florian Frank florian.frank@pingos.org


Linux Software-RAID HOWTO
-------------------------

Beschreibung

Diese HOWTO beschreibt die Benutzung der RAID-Kernelerweiterungen, welche unter Linux den Linear Modus, RAID-0, 1, 4 und 5 als Software-RAID implementieren.

Einführung
^^^^^^^^^^

Ziel dieses Dokumentes ist es, das grundlegende Verständnis der unterschiedlichen RAID-Möglichkeiten und das Erstellen von RAID- Verbunden anhand der - teilweise - neuen Möglichkeiten des 2.2er Kernels zu erklären. Des weiteren wird auf die Besonderheiten mehrerer RAID-Verbunde, die Nutzung dieser als Root-Partition und deren Verhalten bei Fehlern eingegangen. Zu guter Letzt finden Sie noch einige Tipps & Tricks rund um Linux allgemein sowie Software-RAID im speziellen.

Warnung
"""""""

Dieses Dokument beinhaltet keine Garantie für das Gelingen der hier beschriebenen Sachverhalte. Obwohl alle Anstrengungen unternommen wurden, um die Genauigkeit der hier dokumentierten Informationen sicherzustellen, übernimmt der Autor keine Verantwortung für Fehler jeglicher Art oder für irgendwelche Schäden, welche direkt oder als Konsequenz durch die Benutzung der hier dokumentierten Informationen hervorgerufen werden.

RAID, obwohl es dafür entwickelt wurde, die Zuverlässigkeit des Systems zu steigern, indem es Redundanz gewährleistet, kann auch zu einem falschen Gefühl der Sicherheit führen, wenn es unsachgemäß benutzt wird. Dieses falsche Vertrauen kann dann zu wesentlich größeren Desastern führen. Im einzelnen sollte man beachten, dass RAID konstruiert wurde, um vor Festplattenfehlern zu schützen und nicht vor Stromunterbrechungen oder Benutzerfehlern. Stromunterbrechungen, instabile Entwicklerkernel oder Benutzerfehler können zu unwiederbringlichen Datenverlusten führen! RAID ist kein Ersatz für ein Backup Ihres Systems.

Begrifflichkeiten
"""""""""""""""""

Auf den folgenden Seiten werden Sie mit vielen Ausdrücken rund um Software-RAID, Festplatten, Partitionen, Tools, Patches und Devices bombardiert. Um als RAID-Einsteiger mit den oft gebrauchten Ausdrücken nicht ins Schleudern zu geraten, erhalten Sie hier eine Einführung in die Begrifflichkeiten.

Chunk-Size

    Eine genaue Beschreibung, was die Chunk-Size ist, ist im Abschnitt  Spezielle Optionen der RAID-Devices zu finden. 

Devices

    Devices sind unter Linux Stellvertreter für Geräte aller Art, um sie beim Namen nennen zu können. Sie liegen alle unter /dev/ in Ihrem Linux-Verzeichnisbaum. Beispiel dafür sind /dev/hda für die erste (E)IDE-Festplatte im System (analog /dev/hdb, /dev/hdc), /dev/sda für die erste SCSI-Festplatte oder /dev/fd0 für das erste Diskettenlaufwerk. 

Festplatten

    Festplatten sollten Ihnen bekannt sein. RAID nutzt mehrere Festplatten, um entweder deren Gesamtgeschwindigkeit, deren Sicherheit oder beides zu erhöhen. 

Hot Plugging

    Hot Plugging bezeichnet die Möglichkeit, einzelne Festplatten ohne ein Abschalten oder Herunterfahren des Rechners innerhalb eines redundanten RAID-Verbundes abzuschalten, auszutauschen und wieder einzufügen. Im günstigsten Fall bietet einem Hot Plugging also die Möglichkeit, eine defekte Festplatte auszutauschen, ohne den Rechner neu starten zu müssen und ohne die Verfügbarkeit Ihres Servers zu beeinträchtigen. Für die Benutzer würde ein Austausch einer defekten Festplatte absolut transparent erfolgen. 

MD-Device

    MD steht für Multiple-Disk oder Multiple-Device und bedeutet dasselbe wie ein RAID-Device. Um Sie nicht weiter in die Irre zu führen, wird im folgenden auf die Bezeichnung MD-Device bewusst verzichtet. 

MD-Tools

    Die MD-Tools sind Hilfsprogramme, die Ihnen die Möglichkeit zur Einrichtung oder änderung von RAID-Verbunden zur Verfügung stellen, welche mit denen im Standardkernel Version 2.0.x oder 2.2.x enthaltenen RAID-Treibern erstellt wurden. Sie beinhalten als MD-Tools bis einschließlich Version 0.4x für ältere RAID-Verbunde dieselbe Grundfunktionalität wie die RAID-Tools Version 0.9x für die aktuellen RAID-Treiber. Ihre Entwicklung ist zwar eingestellt worden, dennoch sind sie auf vielen älteren Linux-Distributionen vertreten. Die einzelnen Programme dieses MD-Tools Paketes zur Verwaltung von älteren RAID-Verbunden der Version 0.4x werden im entsprechenden Abschnitten abgehandelt. 

md0

    /dev/md0 ist ein Stellvertreter für den erste RAID-Verbund in Ihrem System. Das Verzeichnis /dev/ zeigt an, dass es sich um ein Device handelt, md meint ein Multiple-Disk oder Multiple-Device und damit einen Verbund aus mehreren Partitionen Ihrer Festplatte(n). Das erste Device jeglicher Art ist immer entweder mit einer 0 gekennzeichnet und wird weiter aufsteigend nummeriert (also /dev/md0, /dev/md1, usw.) oder beginnt mit /dev/hda und wird alphabetisch aufsteigend durchgezählt (/dev/hdb, /dev/hdc, usw.). 

Partitionen

    Partitionen bezeichnen die Einteilung Ihrer Festplatte in mehrere Segmente. RAID-Verbunde können aus mehreren Partitionen derselben Festplatte oder aus mehrere Partitionen verschiedener Festplatten bestehen. 

Persistent-Superblock

    Eine Beschreibung, was ein Persistent-Superblock ist, ist im Abschnitt   Weitere Optionen des neuen RAID-Patches nachzulesen. 

RAID-Device

    RAID-Device ist die Bezeichnung für einen neu erstellten RAID-Verbund, der jetzt unter einem eigenen Namen anzusprechen ist. Unter Linux entspricht jedes Gerät letztendlich einem Device. Die RAID-Devices sind im Linux-Verzeichnisbaum unter /dev/ abgelegt und heißen md0-15. 

RAID-Partition

    RAID-Partition bezeichnet eine einzelne Festplattenpartition, die für die Verwendung in einem RAID-Verbund genutzt werden soll. 

RAID-Patch

    Der RAID-Patch bezeichnet ein Paket aktueller RAID-Treiber, die neuer als die im Standardkernel enthaltenen sind. Sie weisen in ihrem Namen eine Zeichenfolge auf, die sich mit der von Ihnen verwendeten Kernel-Version decken sollte. Z.B. braucht der Kernel 2.2.10 den RAID-Patch für den 2.2.10er Kernel. Dieser RAID-Patch aktualisiert also im Endeffekt die originalen RAID-Treiber in Ihrem Kernel-Sourcetree. 

RAID-Tools

    Die RAID-Tools sind Hilfsprogramme, um den Umgang mit RAID-Verbunden in Form von Einrichtung, Wartung und änderung überhaupt zu ermöglichen. Sie existieren für die im Standardkernel 2.0.x und 2.2.x vorhandenen RAID-Treiber als MD-Tools in der Version 0.4x, für die aktuellen RAID-Treiber als RAID-Tools mit der Versionsnummer 0.9x. Der Funktionsumfang der RAID-Tools Version 0.9x überwiegt gegenüber dem der MD-Tools und erlaubt einen einfacheren Umgang mit den RAID-Systemen. Sie sollten bei der Verwendung der RAID-Tools prüfen, ob sie auch die passende Version haben. 

RAID-Verbund

    RAID-Verbund, oder synonym RAID-Array ist die Bezeichnung für eine Zusammenfügung von mehrerer Partitionen einzelner Festplatten, um sie nachher als eine Einheit ansprechen zu können. Ein anschauliches Beispiel wären zwei Partitionen jeweils einer Festplatte, welche als eine komplett neue Partition und damit als ein neuer RAID-Verbund zur Verfügung stehen würden. über den Stellvertreter /dev/md0 würde dieser RAID-Verbund auf Linux Ebene als ein RAID-Device angesprochen werden, der als eine einzelne Zuweisung für zwei darunterliegende Partitionen fungiert. 

Redundanz

    Redundanz kommt aus dem Lateinischen, bedeutet überfülle und meint auf einen RAID-Verbund bezogen das Vorhandensein zusätzlicher Kapazitäten, die keine neuen Daten enthalten, also speziell das ein Datenträger ausfallen kann, ohne den vorhandenen Datenbestand zu beeinträchtigen. 

Spare-Disk

    Informationen hierzu sind im Abschnitt   Weitere Optionen des neuen RAID-Patches zu finden. 


Literatur
"""""""""

Obwohl in dieser HOWTO alles nötige Wissen zum Erstellen von Software-RAID Verbunden unter Linux vermittelt werden sollte, sei hier Trotzdem zum besseren Verständnis und als weiterführende Texte auf folgende HOWTOs und entsprechende Fachliteratur verwiesen:

    BootPrompt HOWTO
    Kernel HOWTO
    Root-RAID HOWTO
    Software-RAID mini-HOWTO
    The Software RAID HOWTO


Wer hat dieses Dokument zu verantworten?
""""""""""""""""""""""""""""""""""""""""

Niels Happel hat zwar die Informationen zusammengetragen, getestet und neu geschrieben, jedoch beruht der größte Teil des Erfolgs dieser HOWTO natürlich auf der Arbeit der Programmierer der RAID-Kernelerweiterungen. Mein besonderer Dank gilt denen, die mir teilweise mit vielen Anregungen, Tipps, Texten und Unermüdlichkeit weitergeholfen haben und allen anderen, die mich dahingehend unterstützt haben. Die fleißigsten waren:

    Uwe Beck (  ubeck@debis.com)
    Robert Dahlem (  robert.dahlem@gmx.net)
    Ulrich Herbst (  ulrich.herbst@debis.com)
    Werner Modenbach (  modenbach@alc.de)


Copyright
"""""""""

Dieses Dokument ist urheberrechtlich geschützt. Das Copyright für dieses Dokument liegt bei Niels Happel und Marco Budde.

Das Dokument darf gemäß der GNU General Public License verbreitet werden. Insbesondere bedeutet dieses, dass der Text sowohl über elektronische wie auch physikalische Medien ohne die Zahlung von Lizenzgebühren verbreitet werden darf, solange dieser Copyright-Hinweis nicht entfernt wird. Eine kommerzielle Verbreitung ist erlaubt und ausdrücklich erwünscht. Bei einer Publikation in Papierform ist das Deutsche Linux HOWTO Projekt hierüber zu informieren.

Info
""""

Wenn Sie irgendwelche Ideen zu dieser HOWTO haben, positive wie negative Kritiken, Korrekturen, oder Wünsche für die nächste Version ... eine E-Mail ist wirklich schnell geschrieben.

    Niels Happel
    E-Mail: (  happel@resultings.de) 

Die jeweils aktuellste Version finden Sie unter:

    en http://www.resultings.de/linux/
    de http://www.tu-harburg.de/dlhp/


Was bedeutet RAID?
^^^^^^^^^^^^^^^^^^

RAID steht entweder für Redundant Array of Independent Disks oder für Redundant Array of Inexpensive Disks und bezeichnet eine Technik, um mehrere Partitionen miteinander zu verbinden. Die beiden verschiedenen Bezeichnungen und teilweise auch die Grundidee von RAID-Systemen rühren daher, dass Festplattenkapazität zu besitzen früher eine durchaus enorm kostspielige Angelegenheit werden konnte. Kleine Festplatten waren recht günstig, große Festplatten aber unangemessen teuer. Man suchte also eine Möglichkeit, mehrere Festplatten kostengünstig zu einer großen zu verbinden. Aufgrund der plötzlichen sprunghaften Erweiterung der erreichbaren Festplattenkapazitäten und der unaufhaltsam sinkenden Preise pro Megabyte Festplattenplatz hat sich das Erreichen des damaligen wichtigsten Zieles eines RAID-Verbundes von selbst erledigt. Dennoch war man gewillt, den bis dahin erzielten Entwicklungsstand eines RAID-Systems auch anderweitig zu nutzen.

Das Ziel eines RAID-Verbundes ist heutzutage deshalb entweder eine Performancesteigerung, eine Steigerung der Datensicherheit oder eine Kombination aus beidem. RAID kann vor Festplattenfehlern schützen und kann auch die Gesamtleistung im Gegensatz zu einzelnen Festplatten steigern.

Dieses Dokument ist eine Anleitung zur Benutzung der Linux RAID-Kernelerweiterungen und den dazugehörigen Programmen. Die RAID-Erweiterungen implementieren den Linear (Append) Modus, RAID-0 (Striping), RAID-1 (Mirroring), RAID-4 (Striping & Dedicated Parity) und RAID-5 (Striping & Distributed Parity) als Software-RAID. Daher braucht man mit Linux Software RAID keinen speziellen Hardware- oder Festplattenkontroller, um viele Vorteile von RAID nutzen zu können. Manches lässt sich mit Hilfe der RAID-Kernelerweiterungen sogar flexibler lösen, als es mit Hardwarekontrollern möglich wäre.

Folgende RAID Typen werden unterschieden:

Linear (Append) Mode

    Hierbei werden Partitionen unterschiedlicher Größe über mehrere Festplatten hinweg zu einer großen Partition zusammengefügt und linear beschrieben. Hier ist kein Geschwindigkeitsvorteil zu erwarten. Fällt eine Festplatte aus, so sind alle Daten verloren. 

RAID-0 (Striping) Mode

    Auch hier werden zwei oder mehr Partitionen zu einer großen zusammengefügt, allerdings erfolgt hier der Schreibzugriff nicht linear (erst die 1.Platte bis sie voll ist, dann die 2.Platte usw.), sondern parallel. Dadurch wird ein deutlicher Zuwachs der Datenrate insbesondere bei SCSI-Festplatten erzielt, welche sich für die Dauer des Schreibvorgangs kurzfristig vom SCSI Bus abmelden können und ihn somit für die nächste Festplatte freigeben. Die erzielten Geschwindigkeitsvorteile gehen allerdings zu Lasten der CPU Leistung. Bei einer Hardware RAID Lösung würde der Kontroller diese Arbeit übernehmen. Allerdings steht der Preis eines guten RAID Kontrollern in keinem Verhältnis zur verbrauchten CPU Leistung eines durchschnittlichen Computers mit Software-RAID. 

RAID-1 (Mirroring) Mode

    Der Mirroring Mode erlaubt es, eine Festplatte auf eine weitere gleichgroße Festplatte oder Partition zu duplizieren. Dieses Verfahren wird auch als Festplattenspiegelung bezeichnet. Hierdurch wird eine erhöhte Ausfallsicherheit erreicht - streckt die eine Festplatte die Flügel, funktioniert die andere noch. Allerdings ergibt das auch wieder nur Sinn, wenn die gespiegelten Partitionen auf unterschiedlichen Festplatten liegen. Die zur Verfügung stehende Festplattenkapazität wird durch dieses Verfahren halbiert. Ein Geschwindigkeitsgewinn ist hierbei nur beim Lesezugriff zu erwarten, jedoch erbringt der aktuelle Stand der RAID-Treiber für Linux nur beim nicht sequentiellen Lesen vom RAID-1 Geschwindigkeitsvorteile. 

RAID-4 (Striping & Dedicated Parity) Mode

    RAID-4 entspricht dem RAID-0 Verfahren, belegt allerdings eine zusätzliche Partition mit Paritätsinformationen, aus denen eine defekte Partition wieder hergestellt werden kann. Allerdings kostet diese Funktion wieder zusätzliche CPU Leistung. 

RAID-5 (Striping & Distributed parity)Mode

    Hier werden die Paritätsinformationen zum Restaurieren einer defekten Partition zusammen mit den tatsächlichen Daten über alle Partitionen verteilt. Allerdings erkauft man sich diese erhöhte Sicherheit durch einen Kapazitätsverlust. Will man 5x1 GB zu einem RAID-5 zusammenfassen, so bleiben für die eigentlichen Daten noch 4x1 GB Platz übrig. Beim Schreibvorgang auf einen RAID-5 Verbund wird erst ein Datenblock geschrieben, dann erfolgt die Berechnung der Paritätsinformationen, welche anschließend auch auf den RAID-Verbund geschrieben werden. Hierher rührt die schlechtere Schreibgeschwindigkeit der Daten. Der Lesevorgang ähnelt allerdings dem RAID-0 Verbund. Das Resultat ist deshalb eine Steigerung der Lesegeschwindigkeit im Gegensatz zu einer einzelnen Festplatte. 

RAID-10 (Mirroring & Striping) Mode

    RAID-10 bezeichnet keinen eigenständigen RAID-Modus, sondern ist ein Kombination aus RAID-0 und RAID-1. Hierbei werden zuerst zwei RAID-0 Verbunde erstellt, die dann mittels RAID-1 gespiegelt werden. Der Vorteil von einem RAID-10im Gegensatz zu einem RAID-5 ergibt sich aus der höheren Performance. Während ein RAID-5 nur relativ wenig Geschwindigkeitsvorteile bietet, ist ein RAID-10 durch die beiden RAID-0 Verbunde für den Fall besser geeignet, wenn man sowohl Redundanz als auch einen hohen Geschwindigkeitsvorteil erzielen will. Sogar die anschließend notwendige RAID-1 Spiegelung bringt noch einen Vorteil bei der Lesegeschwindigkeit. Weiterhin fällt hierbei die notwendige Berechnung von Paritätsinformationen weg. Erkauft wird dies allerdings durch eine sehr viel schlechtere Nutzung des vorhandenen Festplattenplatzes, da immer nur 50% der tatsächlichen Kapazität mit den eigentlichen Daten beschrieben werden kann. 

Voraussetzungen
^^^^^^^^^^^^^^^

Hardware
""""""""

Zum Erstellen von RAID-Devices werden für den Linear, RAID-0 und RAID-1 Modus mindestens zwei leere Partitionen auf möglichst unterschiedlichen Festplatten benötigt. Für RAID-4 und RAID-5 sind mindestens drei Partitionen nötig und für den RAID-10 Modus vier. Dabei ist es egal, ob die Partitionen auf (E)IDE- oder SCSI-Festplatten liegen.

Will man Software-RAID mit (E)IDE-Festplatten benutzen, so empfiehlt es sich, jeweils nur eine (E)IDE-Festplatte an einem (E)IDE Kontroller zu benutzen. Im Gegensatz zu SCSI beherrschen (E)IDE-Festplatten keinen Disconnect - können sich also nicht vorübergehend vom BUS abmelden - und können dementsprechend nicht parallel angesprochen werden. An zwei unterschiedlichen (E)IDE Kontrollern ist dies jedoch in Grenzen möglich, wenn auch immer noch nicht so gut wie bei SCSI Kontrollern. Besser als an einem Strang ist es aber auf jeden Fall.

Des weiteren können Sie die überlegung, Hot Plugging mit (E)IDE-Festplatten zu benutzen, gleich wieder ad acta legen. Hierbei gibt es mehr Probleme als Nutzen. Wie das jedoch mit eigenen (E)IDE RAID Kontrollern aussieht, kann ich mangels passender Hardware nicht sagen.

SCSI Kontroller werden entweder als PCI Steckkarte zusätzlich in den Computer eingebaut, oder sind in Form eines SCSI Chips bereits auf dem Mainboard integriert. Letztere so genannte On Board Controller" hatten bei älteren Mainboards die unangenehme Eigenart, die Interrupt Leitungen für den AGP Steckplatz und den SCSI Chip zusammenzulegen und waren so die Ursache für manche Konfigurations-, Stabilitäts- und daraus resultierend auch Geschwindigkeitsprobleme. Inzwischen funktionieren Mainboards mit integriertem SCSI Chip meist ebenso gut wie externe SCSI Kontroller und stehen in ihrer Leistung einander in nichts nach. Aktuelle Probleme können lediglich aus der mangelnden Treiberunterstützung der verwendeten Distribution resultieren und auf diese Weise eine Linux Installation erschweren.

Das Software-RAID unter Linux bietet zwar die Möglichkeit, einzelne Partitionen einer Festplatte in einen RAID-Verbund einzubauen, um jedoch einen nennenswerten Geschwindigkeitsvorteil oder entsprechende Redundanz zu erzielen, sind generell die RAID-Partitionen auf unterschiedliche Festplatten zu verteilen. Sinnvoll wäre es, das erste RAID zum Beispiel auf drei Partitionen unterschiedlicher Festplatten zu legen; /dev/md0 bestünde dann z.B. aus/dev/sda1, /dev/sdb1 und /dev/sdc1. Das zweite RAID würde man ebenso verteilen: /dev/md1 wäre dann also ein Verbund aus /dev/sda2, /dev/sdb2 und /dev/sdc2. Damit hat man zwei RAID-Verbunde (/dev/md0 und /dev/md1), die sich - als einzelnes Device angesprochen -jeweils über alle drei Festplatten ziehen.

Da es so gut wie immer unsinnig ist, innerhalb eines RAID-Verbundes mehrere Partitionen auf dieselbe Festplatte zu legen (z.B. /dev/md0 bestehend aus /dev/sdb1, /dev/sdb2 und /dev/sdc1), kann oft der Begriff RAID-Festplatte mit RAID-Partition synonym verwendet werden.

Software
""""""""

Die Prozedur wird hier zum einen mit Hilfe der alten RAID-Tools Version 0.4x anhand einer DLD 6.0 und DLD 6.01 beschrieben, zum anderen wird ein aktueller Installationsablauf erläutert, der sich auf den neuen RAID-Patch und die RAID-Tools Version 0.9x bezieht. Der neue Weg hält sich an den Kernel 2.2.10 in Verbindung mit dem passenden RAID-Patch und den RAID-Tools und sollte unabhängig von der von Ihnen benutzten Linux-Distribution funktionieren. Dieser Weg gilt vom Ablauf her auch für alle neueren Kernel der 2.2.xer Reihe. In die Kernel der 2.4.xer Reihe hat der neue RAID-Patch bereits Einzug in den Standard Kernel-Sourcetree erlangt, wodurch diese Kernel nicht mehr aktualisiert werden müssen.

Im allgemeinen ist, vor allem durch die Tatsache, dass die 2.4.xer Kernel nicht mehr gepatcht werden müssen, die Benutzung des neuen RAID-Patches dringend den alten MD-Tools vorzuziehen. Allein der unter Linux sonst unüblich große Versionssprung von 0.4x auf 0.9x zeigt die starken einhergegangenen Veränderungen. Allerdings kann auch das Verfahren mit Hilfe der MD-Tools für ältere Distributionen von Vorteil sein, in denen diese bereits vorkonfiguriert sind oder für eine Situation, in der bereits mit den älteren MD-Tools Version 0.4x eingerichtete RAID-Verbunde vorhanden sind.

Generell sei hier schon gesagt, dass die Lösung durch die MD-Tools und die mit dem aktuellen RAID-Patch unterschiedliche Wege gehen. Bitte beachten Sie das bei der Ihnen vorliegenden Distribution und entscheiden Sie sich frühzeitig für eine Variante. Glauben Sie, zusammen mit einer guten Anleitung mit fdisk und patch zurecht zu kommen, so wählen Sie ruhig den neuen Weg.

Für den aktuellen Abschnitt, welcher die Verwendung der neuen RAID-Kernelerweiterungen Version 0.9x beschreibt, werden neben einem lauffähigen Linuxsystem zwei Archive benötigt. Zum einen der zur Kernelversion passende RAID-Patch und zum anderen die aktuellsten RAID-Tools. Beide Archive gibt es im Internet:

    ftp.//ftp.kernel.org:/pub/linux/daemons/raid/alpha/ 

Aktuelle Distributionen, welche bereits auf einem 2.4.xer Kernel basieren, enthalten neben den aktuellen RAID-Treibern meistens auch ein RPM-Paket mit den aktuellen RAID-Tools. Im Falle einer RedHat 7.2 Linux Distribution heißt das entsprechende Paket raidtools-0.90-23.i386.rpm und ist bei einer Standardinstallation bereits eingerichtet. Der gesamte Einrichtungsteil zur Kernel-Aktualisierung fällt dann angenehmer Weise weg. Alles andere funktioniert allerdings genauso wie mit dem als Beispiel angeführten Kernel 2.2.10.

Generell aktualisiert der RAID-Patch Ihren vorhandenen Linux Kernel-Sourcetree wobei die RAID-Tools die zur Verwaltung von RAID-Verbunden benötigten Programme zur Verfügung stellen. Die einzelnen Programme oder Kommandos der aktuellen RAID-Tools vom 24.08.1999 werden hier mit einer kurzen Erläuterung zu Ihrem Verwendungszweck beschrieben:

ckraid

    Dieses Programm testete in älteren Software-RAID Versionen, die noch mit den MD-Tools Version 0.4x erstellt wurden, die Konsistenz eines RAID-Verbundes. Mit dem aktuellen Kernel-Patch übernimmt der Linux-Kernel diese Arbeit und behandelt die RAID- Verbunde genauso wie alle anderen Partitionen. Dieses Programm ist aus Gründen der Rückwärtskompatibilität noch vorhanden. 

mkraid

    Dies ist das zentrale Verwaltungsprogramm, um RAID-Verbunde aller RAID-Modi anhand einer Konfigurationsdatei - meist /etc/raidtab - zu initialisieren, erstellen oder upzugraden. 

raid0run

    Aus Gründen der Rückwärtskompatibilität kann man mit Hilfe dieses Kommandos Linear und RAID-0 Verbunde starten, welche noch mit den alten MD-Tools Version 0.4x erstellt wurden. 

raidhotadd

    Hiermit wird das sogenannte Hot Plugging, in diesem Fall das Hinzufügen einer RAID-Partition in einen laufenden RAID-Verbund, ermöglicht. 

raidhotremove

    Dies ermöglicht in Analogie zum Kommando raidhotadd das Entfernen einer RAID-Partition aus einem aktiven RAID-Verbund. 

raidsetfaulty

    Um raidhotremove z.B. auf ein laufendes RAID-Array mit Spare-Disks anwenden zu können, muss zuerst die zu entnehmende Festplatte als defekt markiert werden. Ist dies nicht von alleine korrekt geschehen, muss das Kommando raidsetfaulty dazu bemüht werden. Andernfalls erhält man von raidhotremove lediglich eine Fehlermeldung. 

raidstart

    Ist ein RAID-Verbund erst einmal initialisiert, kann er mit diesem Programm gestartet werden. Durch den neuen RAID-Patch und mit den entsprechenden Optionen in der /etc/raidtab kann dies allerdings der Kernel beim Startup des Rechners bereits automatisch erledigen. 

raidstop

    Erlaubt das Deaktivieren eines RAID-Verbundes, um z.B. den Rechner sicher herunterfahren zu können. Auch dies lässt sich mit den nötigen Einträgen in der /etc/raidtab automatisch durch den Kernel erledigen. 

raidtab

    Dies ist die zentrale Konfigurationsdatei für die gesamten RAID-Verbunde Ihres Systems, die erst neu erstellt werden muss. Die Parameter für die einzelnen RAID-Verbunde werden in den entsprechenden Kapiteln dieser HOWTO beschrieben. Standardmäßig suchen die RAID-Tools nach /etc/raidtab. Hier sollte also diese Datei auch erst mal mit diesem Namen erstellt werden. 

Generelles zum Umgang mit Linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Möglichkeiten des Bootens von Linux
"""""""""""""""""""""""""""""""""""

Linux kann auf vielfältige Weise gebootet werden. Gerade im Umgang mit zu testenden RAID-Verbunden und spätestens bei dem Versuch das Root-Verzeichnis auf einen RAID-Verbund verlegen zu wollen, stellt sich einem die Frage, ob und wie Linux bei einem Misserfolg wieder zum sauberen Startup zu bewegen ist. Je nach Zielvorstellung sind dafür unterschiedlich trickreiche Wege zu verfolgen. Diese zahlreichen Möglichkeiten sollen hier erläutert werden. Welche nun speziell für einen beschriebenen RAID-Verbund nötig ist, wird nochmals in den jeweiligen Kapiteln genannt.

Linux von der Festplatte booten

So normal sich das Booten von der Festplatte auch anhört, so treten doch gerade in Verbindung mit RAID-Verbunden als Root-Partition einige Probleme zu Tage die es hierbei zu umschiffen gilt. Andererseits ist es auch oft gerade die Vielfalt der Bootmöglichkeiten, welche den einen oder anderen in Verwirrung stürzt.

DOS Partition mit Loadlin

Ein relativ sicherer und einfacher Weg zugleich Linux zu Booten und schnell die Bootkonfiguration zwischen einem RAID-Verbund und einer normalen ext2-Partition zu wechseln, stellt das Booten per loadlin von einer kleinen DOS-Partition dar. Außer den DOS Systemdateien, einem Linux-Kernel mit RAID-Unterstützung, loadlin und einer Loadlin-Konfigurationsdatei wird nur noch ein DOS Editor benötigt, um simpel die Root-Partition in der Loadlin-Konfigurationsdatei zu ändern.

Extra-Partition für LILO mit Root-RAID

Root-RAID in Verbindung mit LILO braucht noch etwas mehr Fürsorge. Zuerst müssen Sie wissen, ob Ihr LILO im MBR Ihrer Festplatte oder im Superblock Ihrer Root-Partition installiert ist. Ist Linux z.B. das einzige Betriebssystem auf Ihrem Rechner, ist LILO vermutlich im MBR installiert, booten Sie jedoch mittels eines fremden Bootmanagers (OS/2 Bootmanager, XFDisk, oder ähnliche) wird LILO im Superblock Ihrer Root-Partition liegen. Noch einfacher kann das Ihre bisherige /etc/lilo.conf herausstellen: Der Parameter boot= gibt an, wo sich LILO aufhält. Steht dort etwa
/etc/lilo.conf

	   
       boot = /dev/sda

so residiert Ihr LILO im MBR der ersten SCSI-Festplatte, bei der Angabe
/etc/lilo.conf
	   
       boot = /dev/sda2

handelt es sich um den Superblock Ihrer zweiten primären Partition.

LILO braucht zum Booten die Information, wo der Linux-Kernel auf der Festplatte liegt. Da LILO das aber zu einer Zeit erfahren muss, zu der noch gar keine Partition gemountet ist, behilft sich LILO, indem er Plattengeometriedaten in den MBR oder Superblock schreibt, die die genaue Anfangslage des Linux-Kernel beschreiben. Die meisten Distributionen legen Ihre Kernel unter /boot ab. Diesen Umstand kann man nun dahingehend ausnutzen, dass man sich ein kleine Extra-Partition (etwa 10-20 MB) erstellt, welche unterhalb der 1024 Zylindergrenze liegt. Diese formatiert man mit ext2 und mountet sie als /boot in seinen Root-RAID-Device-Verzeichnisbaum, kopiert den gesamten Inhalt von dem originalen /boot Verzeichnis in das neue /boot Verzeichnis und ändert die Dateien /etc/lilo.conf und /etc/fstab dementsprechend:
/etc/lilo.conf
	   
          boot = boot-Partition-ohne-RAID (/dev/sda2),
                 oder: MBR-der-Festplatte (/dev/sda)
          image = /boot/vmlinuz-2.2.10
          root = /dev/md0
          read only

/etc/fstab
	   
        /dev/md0 / ext2 exec,dev,suid,rw 1 1
        /dev/sda2 /boot ext2 exec,dev,suid,rw 1 1

Das Ausführen von lilo sollte dann bescheinigen, dass der Kernel vmlinuz-2.2.10 korrekt initialisiert wurde.

Haben Sie nun LILO im Superblock Ihrer neuen /boot Partition angelegt, so müssen Sie dies noch Ihrem Bootmanager bekannt geben und ihn eben diese booten lassen. Dem Beispiel zufolge wäre das die Partition /dev/sda2. Liegt Ihr LILO im MBR der Festplatte, so brauchen Sie nichts weiter tun, als neu zu booten.

Dieses Verfahren bootet zwar Linux mit einem Root-RAID, ist aber im Fehlerfall der ersten Festplatte nicht redundant!

Extra-Partition für LILO mit redundantem Root-RAID

Die hier beschriebene Vorgehensweise bezieht sich auf die folgende Konstellation: Zwei (E)IDE-Platten sind beide als Master gejumpert und hängen an verschiedenen (E)IDE Kontrollern: /dev/hda und /dev/hdc.

Die Partitionstabelle ist für beide Festplatten gleich:
Partitionstabelle
	   
          /dev/hd?1   primary   Linux native (83)      ca. 10 MB (fuer /boot)
          /dev/hd?2   primary   Linux swap (82)        128 MB (fuer swap)
          /dev/hd?3   primary   Linux raid auto (fd)   den Rest (fuer /dev/md0)

Wenn im folgenden von Backup-Fall gesprochen wird, dann ist damit der Fall gemeint, dass die erste Festplatte ausgefallen ist und irgendwie von der verbliebenen zweiten Festplatte gebootet werden soll.

Wir gehen von folgender /etc/lilo.conf für die erste Festplatte aus:
/etc/lilo.conf
	   
          boot=/dev/hda

          image=/boot/vmlinuz
                  root=/dev/md0
                  label=linux

Nun muss auch auf der zweiten Platte eine Boot-Partition erzeugt werden. Dazu erstellt man auf der zweiten Festplatte eine identische Partition und kopiert mittels einer der im Abschnitt Möglichkeiten zum Kopieren von Daten beschriebenen Methoden das originale Boot-Verzeichnis auf die zweite Festplatte.

Jetzt kopiert man die /etc/lilo.conf der zweiten Festplatte nach /etc/lilo.conf.backup und passt sie an die neuen Bedingungen an. Die endgültige /etc/lilo.conf.backup sollte dann wie folgt aussehen:
/etc/lilo.conf.backup
	   
          boot=/dev/hdc
          disk=/dev/hdc bios=0x80
          map=/boot2/map
          install=/boot2/boot.b

          image=/boot2/vmlinuz
                  root=/dev/md0
                  label=linux
  

Der Parameter disk=/dev/hdc bios=80 ist nötig, um LILO vorzuspiegeln, dass die Festplatte /dev/hdc mit 0x80 eingeloggt ist. Der Grund dafür ist, dass das BIOS normalerweise die ersten beiden Festplatten mit den Adressen 0x80 und 0x81 einloggt. Wir konfigurieren die Platte 0x81 (/dev/hdc). Im Backup-Fall wird die Festplatte aber als 0x80 eingeloggt, da die ursprüngliche erste Festplatte ja defekt ist.

Ein
root@linux # lilo -C /etc/lilo.conf.backup

schreibt die Bootinformationen in den MBR. Es erscheint eine Warnung /dev/hdc is not on the first disk, aber das soll uns nicht stören, denn im Backup-Fall wird diese Festplatte ja zur ersten Festplatte im System. Dafür muss sie natürlich noch an den ersten (E)IDE Kanal gehängt werden.

In komplexeren Fällen ist unter Umständen noch die Optionen ignore-table hilfreich.

Zu bedenken ist noch, dass man nach dem Kompilieren eines neuen Kernels das Boot-Verzeichnis der zweiten Festplatte anpasst und LILO auch mit dem entsprechenden Befehl
root@linux # lilo -C /etc/lilo.conf.backup

für die zweite Festplatte ausführt.

LILO im MBR

Benutzen Sie Linux als einziges Betriebssystem, bietet es sich an, LILO direkt im MBR Ihrer Festplatte unterzubringen.

LILO im Superblock mit externem Bootmanager

Um auch Betriebssysteme neben Linux zu starten, mit denen LILO nicht zurecht kommt, bietet es sich an, einen externen Bootmanager wie etwa den OS/2-Bootmanager oder XFDisk zu benutzen. Hierbei wird LILO im Superblock Ihrer Root-Partition untergebracht und der externe Bootmanager im MBR der Festplatte.

LILO direkt vom RAID im MBR

Das grundsätzliche LILO Problem, die Geometriedaten des Kernels wissen zu müssen und somit nicht direkt von einem RAID-Device booten zu können, kann man umgehen, indem man LILO in der /etc/lilo.conf auf dem Root-RAID diese Parameter schon mit übergibt. Prinzipiell funktioniert dies für alle RAID-Modi. Wirklich Sinn macht das aber nur für RAID-Modi wie RAID-1, RAID-4 und RAID-5, welche auch irgendeine Form von Redundanz versprechen. Im Gegenzug ist das direkte Booten von einem RAID-0-Verbund schon deshalb einfacher zu realisieren, weil man sich beim Defekt einer Festplatte keine Gedanken mehr um die Datenrettung oder das Booten von der zweiten Festplatte zu machen braucht: Diese Daten sind dann ohnehin verloren.

Wie funktioniert nun das direkte Booten von einem RAID-Verbund? Hier ein Beispiel der Datei /etc/lilo.conf für den wohl sinnvollsten RAID-Modus für einen Root-RAID Verbund: RAID-1 auf SCSI-Festplatten:
/etc/lilo.conf
	   
          disk=/dev/md15
                        bios=0x80
                        sectors=63
                        heads=255
                        cylinders=1106
                        partition=/dev/md0
                        start=1028160
          boot=/dev/sda
          image=/boot/vmlinux-2.2.10
                        label=autoraid
                        root=/dev/md0
                        read-only

Der Eintrag disk=/dev/md15 mit seinen Parametern übergibt LILO die benötigten Geometriedaten einer fiktiven Festplatte /dev/md15. Hierbei ist es einerlei, ob dieses Device /dev/md15 oder /dev/md27 heißt. Wichtig ist nur, dass es per mknod mit der Major-Number eines RAID-Devices erstellt wurde. Sind Sie sich nicht sicher, ob dies der Fall ist, lassen Sie sich einfach unter /dev/ alle Devices, die mit md anfangen, zeigen. Standardmäßig werden /dev/md0 bis /dev/md15 erstellt. Die Parameter der Sektoren, Köpfe und Zylinder unterhalb von disk=/dev/md15 geben die Geometriedaten der ersten Festplatte Ihres Systems an, welche man mittels
root@linux # fdisk -lu /dev/sda

erhält. Die Angabe der Partition soll Ihr Root-RAID Verbund sein. Der letzte Parameter start=1028160 bezeichnet den Sektor, in dem Ihre RAID-Partition auf der ersten Festplatte beginnt. Auch diese Information erhalten Sie durch:
root@linux # fdisk -lu /dev/sda

Des weiteren muss als Sitz des LILO der MBR Ihrer ersten Festplatte angegeben werden. Hier geschehen durch den Eintrag: boot=/dev/sda. Der letzte Bereich beschreibt ganz normal die Lokalisation Ihres Kernels mit dem Verweis, als Root-Partition Ihren RAID-Verbund zu nutzen.

Haben Sie den RAID-Verbund nach der weiter unten beschriebenen Methode erstellt und haben sowohl die Option persistent-superblock aktiviert als auch den Partitionstyp der Festplatten auf 0xfd geändert, fehlen dem Master-Boot-Record der SCSI-Festplatten nur noch die Boot-Informationen. Mit dem Aufruf
root@linux # lilo -b /dev/sda

werden die Informationen der Datei /etc/lilo.conf in den MBR der ersten SCSI-Festplatte geschrieben. Anschließend muss man den Befehl ein zweites Mal aufrufen. Diesmal allerdings für den MBR der zweiten Festplatte des RAID-1 Verbundes:
root@linux # lilo -b /dev/sdb

Achtung: Hierbei wird davon ausgegangen, dass die im RAID-Verbund laufenden Festplatten identisch sind und damit auch die gleichen Geometriedaten besitzen! Ein RAID-0 so zu booten, funktioniert auch mit unterschiedlichen Festplatten, da hierbei nur die erste Festplatte berücksichtigt wird. In diesem Beispiel eines RAID-1 liegen jedoch auf allen RAID-Partitionen die gleichen Daten und somit auch die gleiche /etc/lilo.conf. Haben die Festplatten unterschiedliche Geometriedaten und fällt im RAID-1 die erste Festplatte aus, so stehen im MBR der zweiten Festplatte Daten, welche nicht mit denen der zweiten Festplatte übereinstimmen. Ein Workaround könnte sein, zwei LILO Konfigurationsdateien zu pflegen und mit unterschiedlichen Geometriedaten in den MBR der jeweiligen Festplatten zu schreiben. Da mir aber nur mehrere Exemplare dergleichen Festplatte zum Testen von RAID-Verbunden vorliegen, ist dies ein ungesicherter Tipp.

Der Erfolg ist ein RAID-1 Verbund, den man auch nach einem erneuten Kernelkompilierungslauf durch zweimaliges Aufrufen des LILO mit den Parametern für die unterschiedlichen MBRs von beiden beteiligten RAID-Festplatten booten kann.

LILO direkt vom RAID-1 im MBR oder Superblock

Will man die Root-Partition direkt von einem RAID-1 Verbund booten, bietet sich einem noch die weitaus eleganteste Möglichkeit: Die LILO-Version des aktuellen RPM-Archives lilo-0.21-10.i386.rpm kann bereits von sich aus mit RAID-1 Verbunden umgehen. Andere RAID-Modi werden allerdings nicht unterstützt.

Möglichkeiten zum Kopieren von Daten
""""""""""""""""""""""""""""""""""""

Zu den ersten Methoden muss vorab gesagt werden, dass je nach Distribution bei der Verwendung der Root-Partition als RAID einige Verzeichnisse entweder gar nicht kopiert werden dürfen, oder aber nur als leere Verzeichnisse erstellt werden müssen. Im einzelnen sollte man auf folgende achten:

proc

    Dieses Verzeichnis bitte nur als leeres Verzeichnis auf dem RAID-Device erstellen, da unter /proc ein Pseudo-Dateisystem erstellt wird, welches im Prinzip keinen Platz beansprucht - bitte nicht versuchen, /proc auf eine andere Partition zu kopieren. 

mnt

    Dieses Verzeichnis oder das, wo Ihr RAID-Device gemountet ist, darf nicht kopiert werden, sonst würden die bereits vorhandenen Daten nochmals überschrieben werden. 

cdrom

    Die Daten der CDs selbst möchte man natürlich nicht kopieren. Es sollten daher nur die passenden Mountpoints erstellt werden. 

dos

    Auch die DOS-Partition, falls eine vorhanden ist, möchte man nicht mit rüberkopieren. Es sollte also nur ein passender Mountpoint erstellt werden. 

floppy

    Das gleiche gilt auch für eine gemountete Diskette. 

Als generelle Kopiermethoden bieten sich folgende Möglichkeiten an:

cp

    Der normale Copy-Befehl eignet sich für das Kopieren Naheliegenderweise sehr gut und funktioniert problemlos. 

dd

    Auch eine saubere Möglichkeit, Verzeichnisse zu kopieren, bietet das Programm dd. 

dump und restore

    Mittels dump und restore lässt sich ohne viel Aufwand z.B. das ganze Root-Verzeichnis kopieren oder auf Band-Streamer sichern, wobei die unnötigen Verzeichnisse wie /proc oder /mnt fast automatisch ausgelassen werden. Zu diesem Zweck wechselt man in das Verzeichnis, in dem der neue RAID-Verbund gemountet ist und führt die folgenden Befehle aus, um z.B. das Verzeichnis /usr zu kopieren: 

root@linux ~# dump 0Bf 1000000000 - /usr | restore rf -
root@linux ~# rm restoresymtable

Midnight Commander

    Zwar liegt der Midnight Commander zum Kopieren von Verzeichnissen auf ein neues RAID-Array sehr nahe, jedoch haben einige ältere Versionen die bisweilen sehr unangenehme Eigenart, symbolische Links beim Kopieren zu stabilisieren. In den aktuellen Versionen sollte dieses Fehlverhalten jedoch behoben sein. 

tar

    Ebenso zuverlässig und mit einigen Extraoptionen kann man tar benutzen, um ganze Verzeichnisstrukturen zu kopieren. 

Möglichkeiten zum Verändern ganzer Partitionen
""""""""""""""""""""""""""""""""""""""""""""""

ext2resize

    Eine native Möglichkeit unter Linux, ext2-Partitionen zu verändern, die noch dazu der GPL unterliegt, bietet das Programm ext2resize, das von folgender Adresse bezogen werden kann:

    en  http://ext2resize.sourceforge.net/

    Da es aber offiziell noch Beta-Status hat, ist beim Umgang mit diesem Programm Vorsicht geboten. 

Partition Magic

    Seit der Version 4.0 kann Partition Magic auch mit Linux ext2-Partitionen umgehen. Eine Version, die unter Linux selbst lauffähig ist, gibt es allerdings nicht. Das Produkt Partition Magic stammt von der Firma PowerQuest. 

resize2fs

    Dieses Programm ist eine Auskopplung aus Partition Magic, ist unter Linux lauffähig und ermöglicht das Vergrößern und Verkleinern von ext2-Partitionen. Sollten Sie mal über ein gleichnamiges tar-Archiv stolpern, stellt sich hier jedoch noch die Lizenzfrage. Registrierte Benutzer können sich das RPM-Paket von

    en http://www.powerquest.com/

    herunterladen. Innerhalb der Firma überlegt man aber, diesen Teil eventuell frei zu geben. 

RAID-Verbunde mit den RAID-Tools Version 0.4x erstellen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Dieses Kapitel bezieht sich auf die Erstellung von RAID-Verbunden unter Zuhilfenahme der alten MD-Tools Version 0.4x. Die aktuellen RAID-Tools Version 0.9x und der RAID-Patch werden in allen Einzelheiten im Abschnitt  RAID-Verbunde mit den RAID-Tools Version 0.9x erstellen beschrieben.

Vorbereiten des Kernels für 0.4x
""""""""""""""""""""""""""""""""

Der Standardkernel Ihrer Distribution besitzt prinzipiell schon gleich nach der Installation alle Optionen, um RAID-Devices zu erstellen.

Des weiteren ist noch das Paket md-0.35-2.i386.rpm nötig, welches zum Beispiel für die DLD auf der ersten DLD CD unter delix/RPMS/i386/ zu finden ist. Auch wenn dieses Paket bei Ihrer Distribution eine andere Versionsnummer haben sollte, können Sie es ruhig benutzen.

Der Vollständigkeit halber wird hier trotzdem auf die nötigen Kernel-Parameter hingewiesen:

Nach der Anmeldung als root, dem Wechsel in das Verzeichnis /usr/src/linux und dem Aufruf von make menuconfig sollte sich Ihnen das Menü mit den unterschiedlichen Kerneloptionen präsentieren. Bitte benutzen Sie nicht make config oder make xconfig, da sich diese Beschreibung ausschließlich auf make menuconfig stützt.

Kernel 2.0.36

Unter dem Verweis Floppy, IDE, and other block devices ---> werden je nach RAID Wunsch folgende Optionen benötigt:
Floppy, IDE, and other block devices --->
	  
       [*] Multiple devices driver support
       <*> Linear (append) mode
       <*> RAID-0 (striping) mode
       <*> RAID-1 (mirroring) mode
       <*> RAID-4/RAID-5 mode

Kernel 2.2.x bis einschließlich 2.2.10

Hier stehen die RAID Optionen unter Block devices --->:
Block devices --->
	  
       [*] Multiple devices driver support
       <*> Linear (append) mode (NEW)
       <*> RAID-0 (striping) mode (NEW)
       <*> RAID-1 (mirroring) mode (NEW)
       <*> RAID-4/RAID-5 mode (NEW)
       [*] Boot support (linear, striped) (NEW)

Zusätzlich wird bei den 2.2.xer Kerneln bei der Auswahl des Linear oder RAID-0 Modes der Boot Support angeboten. Von RAID-1,4 und 5 Devices kann mit den hier gegebenen Möglichkeiten nicht gebootet werden. Das funktioniert erst mit dem neuen RAID-Patch; siehe Abschnitt  RAID-Verbunde mit den RAID-Tools Version 0.9x erstellen.

Nach Auswahl der nötigen Parameter erfolgt das Backen des Kernels mittels:
root@linux ~# make dep && make clean && make bzImage

Das Kompilieren und Installieren der Module nicht vergessen:
root@linux ~# make modules && make modules_install

Den Kernel zusammen mit der System.map umkopieren und zu guter Letzt den Aufruf von LILO nicht vergessen. Die Benutzer eines SCSI-Kontrollers müssen noch die initiale RAM-Disk mittels
root@linux ~# mkinitrd /boot/initrd Kernelversion

erstellen, falls der SCSI-Kontroller als Modul eingeladen wird.

Nach einem Neustart hat man nun alle Voraussetzungen erfüllt, um ein RAID-Device zu erstellen.

Erstellen eines RAID-0 Devices mit 0.4x
"""""""""""""""""""""""""""""""""""""""

Die RAID-Arrays oder RAID-Verbunde werden nachher über /dev/md* angesprochen. Bei der DLD 6.0 sind diese Devices bereits eingerichtet. Schauen Sie zur Sicherheit unter /dev/ nach.

Jetzt wird das Erstellen eines RAID-Verbundes anhand eines RAID-0 Devices erklärt. Andere RAID-Modi lassen sich analog erstellen. Zuerst sollte man sich darüber im klaren sein, welche und wie viele Partitionen man zusammenfassen möchte. Diese Partitionen sollten leer sein; eine Einbindung von Partitionen, die Daten enthalten, welche nachher wieder zugänglich sein sollen, ist bisher meines Erachtens nicht möglich. Man sollte sich die Devices und ihre Reihenfolge nicht nur gut merken, sondern besser aufschreiben.

Als Beispiel werden die zwei SCSI-Festplatten /dev/sda und /dev/sdb benutzt. Bei (E)IDE würden sie dann /dev/hda, /dev/hdb heißen. Auf diesen Festplatten liegen nun zwei leere Partitionen im erweiterten Bereich /dev/sda6 und /dev/sdb6, welche zu einem RAID-0 Device zusammengefasst werden sollen.
root@linux ~# mdadd /dev/md0 /dev/sda6 /dev/sdb6

Sind diese Partitionen nicht gleich groß, so ist der zu erwartende Geschwindigkeitsvorteil nur auf dem Bereich gegeben, der von beiden Partitionen abgedeckt wird. Zum Beispiel sind eine 200 MB große und eine 300 MB große Partition als RAID-0 Device nur über die ersten 400 MB doppelt so schnell. Die letzten 100 MB der zweiten Festplatte werden ja nun nur noch mit einfacher Geschwindigkeit beschrieben.

Wieder anders sieht das bei der Einrichtung eines RAID-1 Verbundes aus. Hierbei wird der überschüssige Platz von der größeren Partition nicht genutzt und liegt damit brach auf der Festplatte.

Allgemein heißt das also, dass man für RAID-Devices jeder Art möglichst gleich große Partitionen benutzen sollte. Die Ausnahme bildet der Linear Modus, bei dem es wirklich egal ist, wie groß die einzelnen Partitionen sind.

Nun muss Linux noch erfahren, als was für ein RAID-Device es dieses /dev/md0 ansprechen soll:
root@linux ~# mdrun -p0 /dev/md0

Hierbei steht -p0 für RAID-0. Anschließend muss auf diesem neuen RAID-Device ein Dateisystem erstellt werden:
root@linux ~# mke2fs /dev/md0

Testweise kann man das RAID-Device nun nach /mnt mounten und ein paar kleine Kopieraktionen drüberlaufen lassen:
root@linux ~# mount -t ext2 /dev/md0 /mnt

Hat man an den unterschiedlichen Festplatten jeweils einzelne LEDs, sieht man jetzt schon sehr eindrucksvoll, wie das RAID arbeitet. Alle Daten, die ab jetzt auf /dev/md0 geschrieben werden, nutzen den RAID-0 Modus.

Bevor der Rechner runtergefahren wird, müssen die RAID-Devices jedoch noch gestoppt werden:
root@linux ~# umount /mnt
root@linux ~# mdstop /dev/md0

Automatisierung mit 0.4x
""""""""""""""""""""""""

Um nicht nach jedem Bootvorgang diese Prozedur wiederholen zu müssen, benötigt man eine Datei /etc/mdtab, welche - analog zu /etc/fstab - die Mountparameter enthält. Dies erledigt der Befehl:
root@linux # mdcreate raid0 /dev/md0 /dev/sda6 /dev/sdb6

Dadurch wird die Datei /etc/mdtab erstellt, welche zusätzlich noch eine Prüfsumme enthält und das Aktivieren des RAID-Devices durch den einfachen Befehl
root@linux # mdadd -ar

erlaubt. Nun trägt man das RAID-Device (/dev/md0) noch unter /etc/fstab mit der Zeile
/etc/fstab
	  
       /dev/md0 /mnt ext2 defaults 0 1

ein, wobei natürlich /mnt durch jeden beliebigen Mountpoint ersetzt werden kann. Ein
root@linux ~# mount /mnt

führt nun auch zum Mounten des RAID-0 Devices.

Leider berücksichtigen die Init-Skripte der DLD 6.0 keine RAID-Devices, wodurch man noch einmal auf Handarbeit angewiesen ist. Inwieweit andere Distributionen bereits RAID-Verbunde berücksichtigen, sollten Sie durch einen Blick in die Startskripte feststellen und gegebenenfalls ähnlich den hier abgedruckten Beispielen anpassen.

Zum Starten des RAID-Devices ist es unerlässlich den, Befehl mdadd -ar unterzubringen. Will man nicht von dem RAID-Device booten, so reicht es, den Befehl in die Datei /etc/init.d/bc.fsck_other eine Zeile über den fsck Befehl einzutragen.
/etc/init.d/bc.fsck_other
	  
       #################################################
       # Filesystem check und u.U sulogin bei Problemen
       #################################################
       if [ ! -f /fastboot ]
       then
          mini_text="überprüfe Filesysteme..."
          mini_startup "$mini_text" start
          log_msg_buf=`(

       mdadd -ar

             fsck -R -A -a
             ) 2>&1`
          fsck_result=$?
          old_fsck_result=$fsck_result

       [... Teile gelöscht ...]

       #################################################

Analog sollte der Befehl mdstop -a in die zuletzt ausgeführte Datei nach dem umount Befehl eingetragen werden. Bei der DLD heißt die Datei /etc/init.d/halt und sollte nach dem Eintrag an der richtigen Stelle so aussehen:
/etc/init.d/halt
	  
       #################################################
       # Umount all FS
       #################################################
       mini_text="Die Filesysteme werden gelöst"
       mini_shutdown "$mini_text" start
       LC_LANG=C
       LC_ALL=C
       export LC_LANG LC_ALL
       # Gib den sleeps der busy_wait_loops zeit sich zu beenden.
       # sonst gibt es :/usr device busy.
       sleep 1
       umount -a

       mdstop -a

       mini_shutdown "$mini_text" stop 0
       #################################################

Damit kann man nun recht komfortabel das RAID-Device benutzen.

Weitere RAID-Verbunde erstellt man auf dieselbe Weise, jedoch sind die Einträge in den Init-Skripten nur einmal zu setzen.

RAID-Verbunde mit den RAID-Tools Version 0.9x erstellen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Vorbereiten des Kernel für 0.9x
"""""""""""""""""""""""""""""""

Sie benötigen hierfür den aktuellen, sauberen, ungepatchten Sourcetree des 2.2.10er Kernels. Bitte tun Sie sich selbst einen Gefallen und nehmen Sie keinen Sourcetree Ihrer Distribution. Diese sind meist schon mit Patches aller Art versehen und ein zusätzliches ändern mit dem RAID-Patch würde die Sourcen eventuell sogar zerstören. Die kompletten Kernel-Sourcen erhält man auf:

ftp://ftp.kernel.org/pub/linux/kernel/v.2.2/

Des weiteren benötigen Sie den für diesen Kernel passenden RAID-Patch (raid0145-19990724-2.2.10) und die aktuellsten RAID-Tools (raidtools-19990724-0.90.tar.gz). Diese sind hier zu finden:

ftp://ftp.kernel.org/pub/linux/daemons/raid/alpha/

Bitte achten Sie genau auf die passende Kernelversion!
Zuerst muss der Kernel nach /usr/src/linux kopiert und mittels
root@linux ~# tar xvfz kernelfile.tar.gz

entpackt werden. Alternativ und bei Benutzung mehrerer Kernel entpackt man ihn nach /usr/src/linux-2.2.10 und setzt den vermutlich schon vorhandenen symbolischen Link von /usr/src/linux auf /usr/src/linux-2.2.10. Dies ist für das Patchen wichtig, da sonst der falsche Sourcetree gepatcht werden könnte. Auch ist es immer schlau, sich einen ungepatchten Original-Sourcetree aufzuheben, falls mal etwas schief geht.

Nun wird der RAID-Patch nach /usr/src/linux-2.2.10 kopiert und dort mittels
root@linux ~# patch -p1 < raidpatchfilename

in den Sourcetree eingearbeitet. Es sollten nun etwa 20 Dateien kopiert und teilweise geändert werden.

Nach dem Aufruf von
root@linux ~# make menuconfig

sollte sich Ihnen das Menü mit den unterschiedlichen Kerneloptionen präsentieren. Bitte benutzen Sie nicht make config oder make xconfig, da sich diese Beschreibung ausschließlich auf make menuconfig stützt. Hier aktivieren Sie unter Block devices ---> folgende Optionen:
Block devices --->
	   
       [*] Multiple devices driver support
       [*] Autodetect RAID partitions
       <*> Linear (append) mode
       <*> RAID-0 (striping) mode
       <*> RAID-1 (mirroring) mode
       <*> RAID-4/RAID-5 mode
       < > Translucent mode
       < > Logical Volume Manager support (NEW)
       [*] Boot support (linear, striped)

Da immer wieder einige Fehler durch modularisierte RAID-Optionen auftauchen, nehmen Sie sich an den obigen Einstellungen ein Beispiel und kompilieren Sie den benötigten RAID-Support fest in den Kernel ein.

Passen Sie nun noch alle übrigen Kerneleinstellungen Ihren Wünschen an, verlassen Sie das Menü und kompilieren Sie Ihren neuen Kernel:
root@linux ~# make dep && make clean && make bzImage && make modules
root@linux ~# make modules_install

Die Benutzer eines SCSI-Controllers sollten daran denken, die initiale RAM-Disk mittels
root@linux ~# mkinitrd /boot/initrd Kernelversion

neu zu erstellen, falls der SCSI-Kontroller als Modul eingeladen wird. Anschließend noch den neuen Kernel und die System.map Datei umkopieren, die /etc/lilo.conf bearbeiten, lilo ausführen und das System neu starten.

Ihr Kernel unterstützt nun alle nötigen RAID Optionen.

Weiter geht es mit den RAID-Tools. Entpacken Sie die RAID-Tools z.B. nach /usr/local/src, führen Sie das enthaltene Konfigurationsskript aus und kompilieren Sie die Tools:
root@linux ~# ./configure && make && make install

Die RAID-Tools stellen Ihnen zum einen (unter anderem) die Datei mkraid zur Verfügung und erstellen zum anderen die /dev/md0-15 Devices.

Ob Ihr Kernel die RAID Optionen wirklich unterstützt können Sie mittels
root@linux ~# cat /proc/mdstat

in Erfahrung bringen. Diese Pseudodatei wird auch in Zukunft immer Informationen über Ihr RAID System enthalten. Ein Blick hierher lohnt manchmal. Zu diesem Zeitpunkt sollte zumindest ein Eintrag enthalten sein, der Ihnen zeigt, dass die RAID Personalities registriert sind.

Die RAID-Devices, was dasselbe bezeichnet wie das RAID-Array oder den RAID-Verbund, werden nachher über /dev/md* angesprochen.

Zuerst sollte man sich darüber im klaren sein, welche und wie viele Partitionen man zusammenfassen möchte. Diese Partitionen sollten leer sein und man sollte sich die Devices und ihre Reihenfolge nicht nur gut merken, sondern besser aufschreiben. Eine Einbindung von Partitionen, die Daten enthalten, welche nachher wieder zugänglich sein sollen, ist bisher nur über einen Umweg möglich und das auch nur bei Verwendung eines RAID-0 Verbundes.

Als Beispiel werden die zwei SCSI-Festplatten /dev/sda und /dev/sdb benutzt. Bei (E)IDE heißen sie dann /dev/hda, /dev/hdb usw. Auf diesen Festplatten liegen nun zwei leere Partitionen im erweiterten Bereich /dev/sda6 und /dev/sdb6, welche zu einem RAID-0 Device zusammenfasst werden sollen.

Erstellen eines RAID-0 Devices mit 0.9x
"""""""""""""""""""""""""""""""""""""""

Die Grundkonfiguration der zu erstellenden RAID-Devices werden hier unter /etc/raidtab gespeichert. Diese Datei enthält alle nötigen Angaben für ein oder mehrere RAID-Devices. Generelle Hilfe findet man in den Manual Pages von raidtab und mkraid. Eine raidtab für ein RAID-0 System mit den oben beschriebenen zwei Partitionen müsste so aussehen:
root@linux ~# raiddev /dev/md0
raid-level              0
nr-raid-disks           2
persistent-superblock   1
chunk-size              4

device                  /dev/sda6
raid-disk               0
device                  /dev/sdb6
raid-disk               1

Sind die Eintragungen in Ordnung, kann das RAID mittels
root@linux ~# mkraid /dev/md0

gestartet werden. Formatieren Sie Ihr neues Device:
root@linux ~# mke2fs /dev/md0

Dieses Device kann nun - wie jedes andere Blockdevice auch - irgendwo in den Verzeichnisbaum gemountet werden.

Um das RAID-Device wieder zu stoppen, unmounten Sie es und führen Sie
root@linux ~# raidstop /dev/md0

aus. Nach einem Reboot kann das Device mittels
root@linux ~# raidstart /dev/md0

wieder aktiviert und anschließend überall hin gemountet werden.

Automatisierung mit 0.9x
""""""""""""""""""""""""

Wie viel Komfort einem der neue RAID-Patch bringt, zeigt sich bei diesem Automatisierungsprozess. Das bei der Verwendung der MD-Tools Version 0.4x beschriebene Bearbeiten der Init-Skripte fällt völlig weg. Allerdings ist dafür der einmalige Umgang mit dem nicht ganz ungefährlichen Tool fdisk vonnöten. Bitte seien Sie sich absolut sicher mit der Einteilung Ihrer Festplattenpartitionen, bevor Sie sich damit beschäftigen.

Dieses Beispiel sieht nun vor, den RAID-0 Verbund /dev/md0 bestehend aus /dev/sda6 und /dev/sdb6 komfortabel bei jedem Startup zu mounten und automatisch auch wieder herunterzufahren.

Der Trick liegt hierfür in der von Ihnen im Kernelmenü gewählten Option Autodetect RAID partitions. Der Kernel findet damit bestehende RAID-Arrays beim Booten automatisch und aktiviert sie.

Voraussetzungen dafür sind:

    Autodetect RAID partitions ist im Kernel aktiviert.
    persistent-superblock 1 ist in der Datei /etc/raidtab für Ihr Device gesetzt.
    Der Partitionstyp, der von Ihnen für das RAID genutzten Partitionen, muss auf "0xFD" gesetzt werden.

Sind Sie dieser Anleitung bis hierher gefolgt, dann enthält Ihr Kernel bereits die Autodetection und Sie haben Ihr RAID-Device auch mit dem persistent-superblock Modus erstellt. Bleibt noch das ändern des Partitionstyps.

Stellen Sie sicher, dass das RAID-Device gestoppt ist, bevor Sie anfangen, mit fdisk zu arbeiten:
root@linux ~# umount /dev/md0
root@linux ~# raidstop /dev/md0

Bei der vorliegenden Konfiguration müssten die Typen der Partitionen /dev/sda6 und /dev/sdb6 geändert werden. Dafür wird fdisk zuerst für die erste SCSI-Festplatte aufgerufen:
root@linux ~# fdisk /dev/sda

Man wählt nun die Option t für toggle partition type und wird dann aufgefordert, den Hexcode oder einen Partitionstypen einzugeben. l liefert eine Liste der unterstützten Codes, welche aber im Moment uninteressant ist. Geben Sie nun einfach fd ein und prüfen Sie mit p, ob die Partition tatsächlich geändert wurde. In der Id-Spalte sollte nun fd auftauchen. Beenden Sie anschließend fdisk mit der Option w. Verfahren Sie analog mit den anderen zu Ihrem RAID gehörenden Partitionen. Um z.B. /dev/sdb6 zu ändern, rufen Sie fdisk so auf:
root@linux ~# fdisk /dev/sdb

Haben Sie das erfolgreich hinter sich gebracht, wird Ihnen beim nächsten Startup der Kernel mitteilen, dass er Ihr RAID-Device gefunden hat und es aktivieren. Schauen Sie ruhig noch mal mittels cat /proc/mdstat Ihr Device an. Es sollte nach diesem Neustart bereits aktiviert sein.

Anschließend können Sie Ihr /dev/md0 RAID-Device wie jede andere Partition in die /etc/fstab eintragen und so automatisch an einen beliebigen Ort mounten. Das Dateisystem auf dem RAID ist ext2, obwohl Sie den RAID-Verbund natürlich mit jedem beliebigen Dateisystem formatieren können.

Anmerkung
"""""""""

Manche Distributionen sehen es innerhalb ihrer Init-Skripte bereits vor, eventuell vorhandene RAID-Devices beim Herunterfahren des Systems zu deaktivieren oder aber beim Booten zu aktivieren. Dieser Umstand kann zu Fehlermeldungen führen, die Sie jedoch nicht weiter beeindrucken sollten. Diese Distributionen gehen meist davon aus, dass Sie die alten MD-Tools Version 0.4x benutzen. Ihr gepatchter Kernel übernimmt aber ab sofort das Aktivieren und Deaktivieren Ihrer mit den neuen RAID-Tools erstellten RAID-Devices für Sie. Um die Fehlermeldungen zu unterdrücken, kommentieren Sie einfach in den passenden Init-Skripten (z.B. /etc/rc.d/init.d/halt) die Abfrage nach RAID-Devices und deren nachfolgender Deaktivierung mittels z.B. mdstop -a - oder was auch immer dort angegeben ist - aus. 

Root-Partition oder Swap-Partition als RAID
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Root-Partition als RAID
"""""""""""""""""""""""

Eine existierende HOWTO beschäftigt sich bereits mit dem Thema, allerdings ist Sie erstens auf Englisch und zweitens geht es durch neue Kerneloptionen auch eleganter als dort beschrieben.

Bitte erleichtern Sie sich Ihre Arbeit und senken Sie das Frustniveau, indem Sie vorab noch drei Tipps beherzigen:

    Erstellen Sie ein Backup Ihrer Daten.
    Erstellen Sie sowohl eine funktionierende und getestete DOS als auch Linux Bootdiskette.
    Lesen Sie diese Ausführungen erst mal komplett durch, bevor Sie mittendrin feststellen müssen, etwas wichtiges vergessen zu haben, das Sie zu so spätem Zeitpunkt nicht mehr nachholen können.

DLD 6.0

Für dieses Verfahren wird noch eine DOS oder Win95 Partition in Verbindung mit dem Loadlin benötigt. Loadlin befindet sich - so das Programm nicht schon eingesetzt wird - auf der ersten DLD CD unter /delix/RPMS/i386/loadlin-1.6.2.i386.rpm. Alternativ kann man sich auch eine Bootdiskette anfertigen.

Der Hintergrund ist ganz einfach der, dass LILO mit dem RAID-Device nicht zurechtkommt und man somit nicht explizit von diesem RAID-Device booten kann. Daher behilft man sich hier entweder mit Loadlin, oder aber mit einem Mini Linux auf einer kleinen Extra-Partition. Weitere Möglichkeiten zum Booten von Linux auch von RAID-Verbunden wurden bereits im Abschnitt  Möglichkeiten des Bootens von Linux behandelt.

Nichts desto trotz bleiben wir bei dem Verfahren mit Loadlin. Das Programm befindet sich nach erfolgreicher Installation des RPMS-Paketes unter /dos/loadlin/. Auf der oben genannten nötigen DOS Partition richtet man sich nun ein Verzeichnis wie z.B. Linux ein und kopiert die Dateien loadlin.bat und loadlin.exe zusammen mit dem frischen Kernel, in den die RAID-Parameter einkompiliert wurden, hinein.

Um sicherzugehen, dass auch wirklich nichts passiert, sollte man entweder die nötigen Treiber für den (E)IDE Kontroller oder des passenden SCSI Kontroller auch mit in den Kernel einkompiliert haben.

Die Batchdatei loadlin.bat wird nun dahingehend angepasst, dass wir die Parameter für das zu bootende RAID-Device gleich mit angeben:
linux.bat
	   
       md=&lt;md device no.>,&lt;raid level>,&lt;chunk
       size>,dev0,...,devn

md device no

    Die Nummer des RAID (md) Devices: 1 steht für /dev/md1, 2 für /dev/md2 usw. 

raid level

    Welches RAID Level wird verwendet: -1 für Linear Modus, 0 für RAID-0 (Striping). 

chunk size

    Legt die Chunk Size fest bei RAID-0 und RAID-1 fest. 

dev0-devn

    Eine durch Kommata getrennte Liste von Devices, aus denen das md Device gebildet wird, z.B. /dev/hda1,/dev/hdc1,/dev/sda1. 

Andere RAID-Modi außer Linear und RAID-0 werden im Moment nicht unterstützt. Gemäß der vorher beschriebenen Anleitung würde die Zeile in der linux.bat dann so aussehen:
linux.bat
	   
       c:\linux\loadlin c:\linux\zimage root=/dev/md0
       md=0,0,0,0,/dev/sda6,/dev/sdb6 ro

Dies soll nur eine einzige Zeile sein; außerdem ist auch hier wieder auf die richtige Reihenfolge der Partitionen zu achten. Weiterhin müssen natürlich zwei oder mehrere Partitionen - zu entweder einem RAID-0 oder einem Linear Device zusammengefasst - bereits vorliegen. Der Kernel muss die o.a. Bootoption und die nötigen RAID oder Linear Parameter in den Kernel einkompiliert haben; man beachte: Nicht als Module. Dann mountet man das RAID-Device, welches später die Root-Partition werden soll, nach z.B. /mnt, kopiert mittels einer der im Abschnitt   Möglichkeiten zum Kopieren von Daten beschriebenen Methoden die benötigten Verzeichnisse auf das RAID-Device.

Speziell für die DLD 6.0, aber auch für alle anderen Distributionen sei hier gesagt, dass beim Booten von einem RAID-Device der oben beschriebene Befehl mdadd -ar vor dem ersten mount Befehl auszuführen ist. Für die DLD 6.0 heißt das konkret, dass der Befehl bereits in das Skript /etc/init.d/bc.mount_root eingetragen werden muss, da dort der erste mount Befehl ausgeführt wird. Benutzer anderer Distributionen sind hier auf sich gestellt oder schauen sich zur Not die Methode mit den neuen RAID-Tools Version 0.9x an; siehe Abschnitt   RAID-Verbunde mit den RAID-Tools Version 0.9x erstellen.

Jetzt fehlen nur noch die passenden Einträge in /etc/fstab und /etc/lilo.conf, in denen man auf dem neuen RAID-Device die ursprüngliche Root-Partition in /dev/md0 umändert - im Moment liegen diese Dateien natürlich noch unter /mnt.

An dieser Stelle sollte man die obige Liste noch einmal in Ruhe durchsehen, sich vergewissern, dass alles stimmt, und dann die DOS oder Win95 Partition booten. Dort führt man nun die Batchdatei linux.bat aus.

Generisch

Im Abschnitt  RAID-Verbunde mit den RAID-Tools Version 0.9x erstellen wurde bereits auf die vorzügliche Eigenschaft des automatischen Erkennens von RAID-Devices durch den Kernel-Patch beim Startup des Linuxsystems hingewiesen. Dieser Umstand legt die Vermutung nahe, dass es mit dieser Hilfe noch einfacher ist, die Root-Partition als RAID-Device laufen zu lassen. Das ist auch wirklich so, allerdings gibt es auch hierbei immer noch einige Kleinigkeiten zu beachten. Generell kann man Linux entweder mittels Loadlin oder mit Hilfe von LILO booten. Je nach Bootart ist die Vorgehensweise unterschiedlich aufwendig.

Für beide Fälle braucht man jedoch erst mal ein RAID-Device. Um bei dem Beispiel des RAID-0 Devices mit den Partitionen /dev/sda6 und /dev/sdb6 zu bleiben, nehmen wir dieses Device und mounten es in unseren Verzeichnisbaum.

Hier muss allerdings noch einmal darauf hingewiesen werden, dass ein RAID-0 Device als Root-Partition ein denkbar schlechtes Beispiel ist. RAID-0 besitzt keinerlei Redundanz; fällt eine Festplatte aus, ist das Ganze RAID im Eimer. Für eine Root-Partition sollte man deshalb auf jeden Fall ein RAID-1 oder RAID-5 Device vorziehen. Auch das funktioniert Dank der neuen Autodetect Funktion und wird analog dem beschriebenen RAID-0 Verbund eingerichtet.

Auf das gemountete RAID-Device /dev/md0 kopiert man nun ganz simpel mittels einer der im Abschnitt  Möglichkeiten zum Kopieren von Daten beschriebenen Methoden das komplette Root-Verzeichnis.

Danach muss auf dem RAID-Device noch die Datei /etc/fstab so angepasst werden, das als Root-Partition /dev/md0 benutzt wird und nicht mehr die originale Root-Partition.

Erstellen Sie sich eine DOS-Bootdiskette - das pure DOS von Win95 tut es auch - und auf dieser ein Verzeichnis Linux. Hierher kopieren Sie nun aus dem passenden RPM-Paket Ihrer Distribution das DOS-Tool loadlin und Ihren aktuellen Kernel. Manchmal befindet sich loadlin auch unkomprimiert im Hauptverzeichnis der Distributions-CD. Der Kernel sollte natürlich die RAID-Unterstützung bereits implementiert haben. Nun erstellen Sie mit Ihrem Lieblingseditor in dem neuen Linux Verzeichnis eine loadlin.bat. Haben Sie Ihren Kernel z.B. vmlinuz genannt, sollte in der Datei loadlin.bat etwas in dieser Art stehen:
loadlin.bat
	   
       a:\linux\loadlin a:\linux\vmlinuz root=/dev/md0 ro vga=normal

Die Pfade müssen natürlich angepasst werden. Ein Reboot und das Starten von der Diskette mit der zusätzlichen Ausführung der linux.bat sollte Ihnen ein vom RAID-Device gebootetes Linux bescheren. Booten Sie generell nur über Loadlin, so endet für Sie hier die Beschreibung.

Möchten Sie allerdings Ihr neues Root-RAID mittels LILO booten, finden Sie im Abschnitt  Möglichkeiten des Bootens von Linux diverse Methoden aufgelistet und teilweise sehr genau beschrieben, mit denen Sie sich noch bis zum endgültigen Erfolg beschäftigen müssten.

Anmerkung zum redundanten Root-RAID

Hat man sich bei einer anderen Distribution als SuSE 6.2 für einen Root-RAID Verbund entschieden, der im Fehlerfall auch von der zweiten Festplatte booten soll, muss man noch folgendes beachten: Da auch auf der zweiten Festplatte eine Boot-Partition benötigt wird, die zwar ebenso in der /etc/fstab aufgenommen wurde, aber im Fehlerfall nicht mehr vorhanden ist, fällt die SuSE 6.2 Distribution in einen Notfall-Modus, in dem das root-Paßwort eingegeben werden muss und das fragliche Dateisystem repariert werden soll. Dies kann Ihnen auch bei anderen Distributionen passieren.

Es wird also ein Weg benötigt, die beiden Boot-Partitionen /boot und /boot2 nur dann zu mounten, wenn sie tatsächlich körperlich im Rechner vorhanden sind. Hierbei hilft ihnen das Skript mntboot:
mntboot
	   
  #!/bin/sh

  MNTBOOTTAB=/etc/mntboottab

  case "$1" in
    start)
      [ -f $MNTBOOTTAB ] || {
        echo "$0: *** $MNTBOOTTAB: not found" >&2
        break
      }

      PARTS=`cat $MNTBOOTTAB`

      for part in $PARTS ; do
        [ "`awk '{print $2}' /etc/fstab | grep "^$part$"`" == "" ] && {
          echo "$0: *** Partition $part: not in /etc/fstab" >&2
          continue
        }

        [ "`awk '{print $2}' </proc/mounts | grep "^$part$"`" != "" ] && {
          echo "$0: *** Partition $part: already mounted" >&2
          continue
        }

        fsck -a $part
        [ $? -le 1 ] || {
          echo "$0: *** Partition $part: Defect? Unavailable?" >&2
          continue
        }

        mount $part || {
          echo "$0: *** Partition $part: cannot mount" >&2
          continue
        }
      done

      exit 0
      ;;

    *)
      echo "usage: $0 start" >&2
      exit 1
      ;;
  esac

Das Skript gehört bei SuSE Distributionen nach /sbin/init.d, bei vermutlich allen anderen Linux Distributionen nach /etc/rc.d/init.d. Auf das Skript sollte ein symbolischer Link /sbin/init.d/rc2.d/S02mntboot zeigen. Für alle anderen gilt es hier einen Link nach /etc/rc.d/rc3.d/S02mntboot zu setzen, da außer den SuSE Distributionen wohl alle im Runlevel 3 starten und Ihre Links dafür in diesem Verzeichnis haben. Das Skript prüft ein paar Nebenbedingungen für diejenigen Partitionen, die in /etc/mntboottab eingetragen sind (darin sollten /boot und /boot2 stehen) und ruft jeweils fsck und mount für diese Partitionen auf. Da es bei allen anderen Distributionen keine /etc/mntboottab gibt, gilt es hier diese zu erstellen oder anzupassen.

In der /etc/fstab sollten diese Partitionen mit noauto statt defaults eingetragen werden. Außerdem muss im sechsten Feld der Wert 0 stehen, da die Distribution im Backup-Fall sonst in den Notfall-Modus fällt.

RAID auch für Swap-Partitionen?
"""""""""""""""""""""""""""""""

RAID-Technik mit normalen Swap-Partitionen

Sie überlegen sich, RAID auch für Swap Partitionen einzurichten? Diese Mühe können Sie sich sparen, denn der Linux-Kernel unterstützt ein RAID-Verhalten auf Swap-Partitionen ähnlich dem RAID-0 Modus quasi von Haus aus. Legen Sie einfach auf verschiedenen Festplatten ein paar Partitionen an, ändern Sie den Partitionstyp mittels
root@linux ~# fdisk /dev/Ihre-Partition

und der Option t auf 82 und erstellen Sie das Swap Dateisystem:
root@linux ~# mkswap /dev/Ihre-neue-Swap-Partition

Nun fügen Sie diese in die /etc/fstab ein und geben allen Swap-Partitionen dieselbe Priorität.
fstab
	   
       /dev/hda3 swap swap defaults,pri=1    0 0
       /dev/hdb3 swap swap defaults,pri=1    0 0
       /dev/sda4 swap swap defaults,pri=1    0 0

Vom nächsten Startup an werden die Swap Partitionen wie ein RAID-0 Device behandelt, da die Lese- und Schreibzugriffe ab jetzt gleichmäßig über die Swap-Partitionen verteilt werden.

Will man aus irgendwelchen Gründen zwei Swap-Partitionen höher priorisieren als eine Dritte, so kann man das auch über den Parameter pri= ändern, wobei die Priorität einen Wert zwischen 0 und 32767 annehmen kann. Ein höherer Wert entspricht einer höheren Priorität. Je höher die Priorität desto eher wird die Swap-Partition beschrieben. Bei der folgenden Konfiguration würde also /dev/hda3 wesentlich stärker als Swap-Partition genutzt werden als /dev/hdb3.
fstab
	   
       /dev/hda3 swap swap defaults,pri=5    0 0
       /dev/hdb3 swap swap defaults,pri=1    0 0

Swap-Partitionen auf RAID-1 Verbunden

Erstellt man die Swap-Partition auf einem vorhandenen RAID-1 Verbund, formatiert sie dann mittels mkswap /dev/mdx und trägt sie als Swap-Partition in die /etc/fstab ein, so hat man zwar keinen, oder nur einen kleinen lesenden Geschwindigkeitsvorteil, jedoch den großen, nicht zu unterschätzenden Vorteil, dass man bei einem Festplattendefekt nach dem Ausschalten des Rechners und dem Austausch der defekten Festplatte, ohne weitere manuelle Eingriffe wieder ein vollständig funktionierendes System hat. Der einzige Wermutstropfen betrifft hierbei die Freunde des Hot Plugging. Erfahrungsgemäß verkraftet Linux das Hot Plugging eines dermaßen gestalteten RAID-1 Verbundes nur, wenn vorher die Swap-Partitionen mittels swapoff -a abgeschaltet wurden.

Als Warnung seien hier aber noch zwei der schlimmsten Fälle genannt, über die man sich Gedanken machen sollte und die noch dazu voneinander abhängig sind:

Der erste Fall beschreibt die Situation, Swap auf einem RAID-1 Verbund mit den alten RAID-Tools und damit den sowohl in den 2.0.xer als auch in den 2.2.xer original im Kernel vorhandenen RAID Treibern zu benutzen. Hierbei können nach einer gewissen Laufzeit des Linuxsystems unweigerliche Abstürze auftreten. Der Grund dafür liegt in der Problematik der unterschiedlichen Cachestrategie von Software-RAID einerseits und Swap-Partitionen andererseits. Erst mit den aktuellen RAID-Treibern ist der Betrieb einer Swap-Partition auf einem RAID-Verbund stabil geworden. Wollen Sie also einen sicheren RAID-Verbund erstellen, der nachher aus Gründen der Ausfallsicherheit auch die Swap-Partition beinhalten soll, benutzen Sie bitte immer die aktuellen RAID-Treiber in Form des aktuellen RAID-Patches. Dies entspricht dann der Einrichtfunktionalität der RAID-Tools Version 0.9x.

Der zweite Fall beschreibt die generelle Problematik, mit der Sie sich bei der Einrichtung von Swap-Partitionen in Bezug auf Software-RAID auseinandersetzen müssen:

Hat man die Swap-Partition auf einen RAID-1 Verbund gelegt und zusätzlich dafür eine Spare-Disk reserviert, würde diese Spare-Disk natürlich bei einem Festplattendefekt sofort eingearbeitet werden. Das ist zwar erwünscht und auch so gedacht, jedoch funktioniert das Resynchronisieren dieses RAID-1 Verbundes mit einer aktiven Swap-Partition nicht. Die Software-RAID Treiber nutzen beim Resynchronisieren den Puffer-Cache, die Swap-Partition aber nicht. Das Ergebnis ist eine defekte Swap-Partition.

Als Lösung bleibt nur die Möglichkeit, keine Spare-Disks zu benutzen und nach einem Festplattenausfall swapoff -a per Hand auszuführen, die defekte Festplatte auszutauschen und nach dem Erstellen der Partitionen und des Swap-Dateisystems mit swapon -a wieder zu aktivieren.

Ein Problem bleibt dennoch: Gesetzt den Fall der Linux-Rechner würde aufgrund eines Stromausfalls nicht sauber heruntergefahren worden sein, so werden die RAID-Verbunde beim nächsten Startup automatisch resynchronisiert. Dies erfolgt mit einem automatischen ge-nice-ten Aufruf des entsprechenden RAID-Daemons im Hintergrund bereits zu Anfang der Bootprozedur. Im weiteren Bootverlauf werden aber irgendwann die Swap-Partitionen aktiviert und treffen auf ein nicht synchronisiertes RAID. Das Aktivieren der Swap-Partitionen muss also verzögert werden, bis die Resynchronisation abgeschlossen ist.

Wie unter Linux üblich lässt sich auch dieses Problem mit einem Skript lösen. Der Gedanke dabei ist, den Befehl swapon -a durch ein Skript zu ersetzen, welches die Pseudodatei /proc/mdstat nach der Zeichenfolge resync= durchsucht und im Falle des Verschwindens dieser Zeichenfolge die Swap-Partitionen aktiviert. Im folgenden finden Sie ein Beispiel dazu abgedruckt:
swapon -a
	   
       #!/bin/sh
       #

       RAIDDEVS=`grep swap /etc/fstab | grep /dev/md|cut -f1|cut -d/ -f3`

       for raiddev in $RAIDDEVS
       do
       #  echo "testing $raiddev"
           while grep $raiddev /proc/mdstat | grep -q "resync="
           do
       #     echo "`date`: $raiddev resyncing" >> /var/log/raidswap-status
             sleep 20
            done
            /sbin/swapon /dev/$raiddev
       done

       exit 0
	   
Spezielle Optionen der RAID-Devices Version 0.9x
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Was bedeutet Chunk-Size?
""""""""""""""""""""""""

Mit dem Chunk-Size Parameter legt man die Größe der Blöcke fest, in die eine Datei zerlegt wird, die auf einen RAID-Verbund geschrieben werden soll. Diese ist nicht mit der Blockgröße zu verwechseln, die beim Formatieren des RAID-Verbundes als Parameter eines bestimmten Dateisystems angegeben werden kann. Vielmehr können die beiden verschiedenen Blockgrößen variabel verwendet werden und bringen je nach Nutzung unterschiedliche Geschwindigkeitsvor- wie auch -nachteile.

Nehmen wir das Standardbeispiel: RAID-0 (/dev/md0) bestehend aus /dev/sda6 und /dev/sdb6 soll als angegebene Chunk-Size in der /etc/raidtab 4 kB haben. Das würde heißen, dass bei einem Schreibprozess einer 16 KB großen Datei der erste 4 KB Block und der dritte 4 KB Block auf der ersten Partition (/dev/sda6) landen würden, der zweite und vierte Block entsprechend auf der zweiten Partition (/dev/sdb6). Bei einem RAID, das vornehmlich große Dateien schreiben soll, kann man so bei einer größeren Chunk-Size einen kleineren Overhead und damit einen gewissen Performancegewinn feststellen. Eine kleinere Chunk-Size zahlt sich dafür bei RAID-Devices aus, die mit vielen kleinen Dateien belastet werden. Somit bleibt auch der Verschnitt in einem erträglichen Rahmen. Die Chunk-Size sollte also jeder an seine jeweiligen Bedürfnisse anpassen.

Der Chunk Size Parameter in der /etc/raidtab funktioniert für alle RAID-Modi und wird in Kilobyte angegeben. Ein Eintrag von 4 bedeutet also 4096 Byte. Mögliche Werte für die Chunk-Size sind 4 KB bis 128 KB, wobei sie immer einer 2er Potenz entsprechen müssen.

Wie wirkt sich die Chunk-Size jetzt speziell auf die RAID-Modi aus?

Linear Modus

    Beim Linear Modus wirkt sich die Chunk-Size mehr oder minder direkt auf die benutzten Daten aus. Allgemein gilt hier, bei vielen kleinen Dateien eher eine kleine Chunk-Size zu wählen und umgekehrt. 

RAID-0

    Da die Daten auf ein RAID-0 parallel geschrieben werden, bedeutet hier eine Chunk-Size von 4 KB, dass bei einem Schreibprozess mit einer Größe von 16 KB in einem RAID-Verbund aus zwei Partitionen die ersten beiden Blöcke parallel auf die beiden Partitionen geschrieben werden und anschließend die nächsten beiden wieder parallel. Hier kann man auch erkennen, das die Chunk-Size in engem Zusammenhang mit der verwendeten Anzahl der Partitionen steht. Eine generelle optimale Einstellung kann man also nicht geben. Diese hängt vielmehr vom Einsatzzweck des RAID-Arrays in bezug auf die Größe der hauptsächlich verwendeten Dateien, der Anzahl der Partitionen und den Einstellungen des Dateisystems ab. 

RAID-1

    Beim Schreiben auf ein RAID-1 Device ist die verwendete Chunk-Size für den "parallelen" Schreibprozess unerheblich, da sämtliche Daten auf beide Partitionen geschrieben werden müssen. Die Abhängigkeit besteht hier also wie beim Linear-Modus von den verwendeten Dateien. Beim Lesevorgang allerdings bestimmt die Chunk-Size, wie viele Daten zeitgleich von den unterschiedlichen Partitionen gelesen werden können. Der Witz ist hierbei, dass alle Partitionen dieselben Daten enthalten (Spiegel-Partitionen eben) und dadurch Lesevorgänge wie eine Art RAID-0 arbeiten. Hier ist also mit einem Geschwindigkeitsgewinn zu rechnen. 

RAID-4

    Hier beschreibt die Chunk-Size die Größe der Paritätsblöcke auf der Paritäts-Partition, welche nach dem eigentlichen Schreibzugriff geschrieben werden. Wird auf einen RAID-4 Verbund 1 Byte geschrieben, so werden die Bytes, die die Blockgröße bestimmen, von den RAID-4 Partitionen abzüglich der Paritäts-Partition (X-1, wobei X die Gesamtzahl der RAID-4 Partitionen ist) gelesen, die Paritäts-Information berechnet und auf die Paritäts-Partition geschrieben. Die Chunk-Size hat auf den Lesevorgang denselben geschwindigkeitsgewinnenden Einfluss wie bei einem RAID-0 Verbund, da der Lesevorgang auf eine ähnliche Weise realisiert ist. 

RAID-5

    Beim RAID-5 Verbund bezeichnet die Chunk-Size denselben Vorgang wie beim RAID-4. Eine allgemein vernünftige Chunk-Size läge hier zwischen 32 KB und 128 KB, jedoch treffen auch hier die Faktoren wie Nutzung des RAIDs und verwendete Partitionen auf den Einzelfall zu und die Chunk-Size sollte dementsprechend angepasst werden. 

RAID-10

    Bei dieser Art des RAID-Verbundes bezieht sich die Chunk-Size auf alle integrierten RAID-Verbunde, also beide RAID-0 Verbunde und das RAID-1 Array. Als Richtwert kann man hier 32 KB angeben - experimentieren ist hierbei natürlich wie bei allen anderen RAID-Modi auch ausdrücklich erlaubt. 


Spezielle Optionen von mke2fs für RAID-4 und RAID-5 Systeme
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Die Option -R stride=nn von mke2fs erlaubt es, verschiedene ext2 spezifische Datenstrukturen auf eine intelligentere Weise auf das RAID zu verteilen. Ist die Chunk-Size mit 32 KB angegeben, heißt das, dass 32 KB fortlaufende Daten als Block auf dem RAID-Verbund liegen. Danach würde der nächste 32 KB Block kommen. Will man ein ext2 Dateisystem mit 4 KB Blockgröße erstellen, erkennt man, dass acht Dateisystemblöcke in einem Verbundblock (Chunk Block; durch Chunk-Size angegeben) untergebracht werden. Diese Information kann man dem ext2 Dateisystem beim Erstellen mitteilen:
root@linux ~# mke2fs -b 4096 -R stride=8 /dev/md0

Die RAID-4 und RAID-5 Performance wird durch diesen Parameter erheblich beeinflusst. Das Benutzen dieser Option ist dringend anzuraten.

Beispiel-raidtab Einträge für alle RAID-Modi
""""""""""""""""""""""""""""""""""""""""""""

Hier werden nur der neue RAID-Kernel-Patch in Verbindung mit den passenden RAID-Tools Version 0.9x und der entsprechenden Kernel-Version beschrieben. Die älteren RAID-Tools der Version 0.4x finden keine Berücksichtigung. Der Kernel-Patch sollte installiert sein. Ebenso sollten die RAID-Optionen im Kernel aktiviert sein und der Kernel neu kompiliert und neu gebootet sein.

Linear (Append) Modus

Der Linear Modus verbindet mehrere Partitionen unterschiedlicher Größe zu einem Gesamtverbund, der allerdings nicht parallel, sondern nacheinander beschrieben wird. Ist die erste Partition voll, wird auf der nächsten weitergeschrieben. Dadurch sind weder Performancevorteile zu erwarten noch eine gesteigerte Sicherheit. Im Gegenteil. Ist eine Partition des Linear-Verbundes defekt, ist meist auch der gesamte Verbund hinüber.

Die Parameter der /etc/raidtab für einen Linear-Verbund sehen so aus:
root@linux ~# raiddev /dev/md0
raid-level              linear
nr-raid-disks           2
persistent-superblock   1
chunk-size              4
device                  /dev/sda6
raid-disk               0
device                  /dev/sdb6
raid-disk               1

Zu beachten ist hier, dass der Linear Modus keine Spare-Disks unterstützt. Was das ist und was man unter dem Persistent-Superblock versteht, finden Sie im Abschnitt  Weitere Optionen der neuen RAID-Patches. Der Parameter Chunk-Size wurde bereits in diesem Abschnitt weiter oben erläutert.

Initialisiert wird Ihr neues Device mit folgendem Kommando:
root@linux ~# mkraid -f /dev/md0

Danach fehlt noch ein Dateisystem:
root@linux ~# mke2fs /dev/md0

Schon können Sie das Device überall hin mounten und in die /etc/fstab eintragen.

RAID-0 (Striping)

Sie haben sich entschlossen, mehrere Partitionen unterschiedlicher Festplatten zusammenzufassen, um eine Geschwindigkeitssteigerung zu erzielen. Auf die Sicherheit legen Sie dabei weniger Wert, jedoch sind Ihre Partitionen alle annähernd gleich groß.

Ihre /etc/raidtab sollte daher etwa so aussehen:
root@linux ~# raiddev /dev/md0
raid-level              0
nr-raid-disks           2
persistent-superblock   1
chunk-size              4

device                  /dev/sda6
raid-disk               0
device                  /dev/sdb6
raid-disk               1

Hier werde genauso wie beim Linear Modus keine Spare-Disks unterstützt. Was das ist und was man unter dem Persistent-Superblock versteht, wird im Abschnitt  Weitere Optionen der neuen RAID-Patches erläutert.

Initialisiert wird Ihr neues Device mit folgendem Kommando:
root@linux ~# mkraid -f /dev/md0

Danach fehlt noch ein Dateisystem:
root@linux ~# mke2fs /dev/md0

Schon können Sie das Device überall hin mounten und in die /etc/fstab eintragen.

RAID-1 (Mirroring)

Ein RAID-1 Verbund wird auch als Spiegelsystem bezeichnet, da hier der gesamte Inhalt einer Partition auf eine oder mehrere andere gespiegelt und damit eins zu eins dupliziert wird. Wir haben hier also den ersten Fall von Redundanz. Des weiteren können hier, falls es erwünscht ist, zum ersten mal Spare-Disks zum Einsatz kommen. Diese liegen pauschal erst mal brach im System und werden erst um Ihre Mitarbeit bemüht, wenn eine Partition des RAID-1 Verbundes ausgefallen ist. Spare-Disks bieten also einen zusätzlichen Ausfallschutz, um nach einem Ausfall möglichst schnell wieder ein redundantes System zu bekommen.

Die /etc/raidtab sieht in solch einem Fall inkl. Spare-Disk so aus:
root@linux ~# raiddev /dev/md0
raid-level              1
nr-raid-disks           2
nr-spare-disks          1
chunk-size              4
persistent-superblock   1

device                  /dev/sda6
raid-disk               0
device                  /dev/sdb6
raid-disk               1
device                  /dev/sdc6
spare-disk              0

Weitere Spare- oder RAID-Disks würden analog hinzugefügt werden. Führen Sie hier nun folgendes Kommando aus:
root@linux ~# mkraid -f /dev/md0

Mittels cat /proc/mdstat können Sie wieder den Fortschritt und den Zustand Ihres RAID-Systems erkennen. Erstellen Sie das Dateisystem mittels
root@linux ~# mke2fs /dev/md0

sobald das RAID sich synchronisiert hat.

Theoretisch funktioniert das Erstellen des Dateisystems bereits während sich das RAID-1 noch synchronisiert, jedoch ist es vorläufig sicherer, mit dem Formatieren und Mounten zu warten, bis das RAID-1 fertig ist.

RAID-4 (Striping & Dedicated Parity)

Sie möchten mehrere, aber mindestens drei etwa gleich große Partitionen zusammenfassen, die sowohl einen Geschwindigkeitsvorteil als auch erhöhte Sicherheit bieten sollen. Das Verfahren der Datenverteilung beim Schreibzugriff ist hierbei genauso wie beim RAID-0 Verbund, allerdings kommt hier eine zusätzliche Partition mit Paritätsinformationen hinzu. Fällt eine Partition aus, so kann diese, falls eine Spare-Disk vorhanden ist, sofort wieder rekonstruiert werden; fallen zwei Partitionen aus, ist aber auch hier Schluss und die Daten sind verloren. Obwohl RAID-4 Verbunde eher selten eingesetzt werden, müsste Ihre /etc/raidtab dann so aussehen:
root@linux ~# raiddev /dev/md0
raid-level              4
nr-raid-disks           3
nr-spare-disks          0
persistent-superblock   1
chunk-size              32

device                  /dev/sda6
raid-disk               0
device                  /dev/sdb6
raid-disk               1
device                  /dev/sdc6
raid-disk               2

Möchten Sie Spare-Disks einsetzen, so werden sie analog der Konfiguration des RAID-1 Devices hinzugefügt, also:
nr-spare-disks  1
device          /dev/sdd6
spare-disk      0

Der Grund dafür, dass RAID-4 Verbunde nicht oft eingesetzt werden, liegt daran, dass die Paritäts-Partition immer die gesamten Daten des restlichen - als RAID-0 arbeitenden - Verbundes schreiben muss. Denkt man sich einen Extremfall, wo zehn Partitionen als RAID-0 arbeiten und eine Partition nun die gesamten Paritätsinformationen speichern soll, so wird unweigerlich deutlich, dass die Paritäts-Partition schnell überlastet ist.

Initialisiert wird Ihr neues Device so:
root@linux ~# mkraid -f /dev/md0

Danach fehlt noch ein Dateisystem:
root@linux ~# mke2fs /dev/md0

Schon können Sie das Device überall hin mounten und in die /etc/fstab eintragen.

RAID-5 (Striping & Distributed Parity)

Ein RAID-5 Verbund löst nun das klassische RAID-4 Problem elegant, indem es die Paritätsinformationen gleichmäßig über alle im RAID-5 Verbund laufenden Partitionen verteilt. Hierdurch wird der Flaschenhals einer einzelnen Paritäts-Partition wirksam umgangen, weshalb sich RAID-5 als Sicherheit und Geschwindigkeit bietender RAID-Verbund großer Beliebtheit erfreut. Fällt eine Partition aus, beginnt das RAID-5, falls Spare-Disks vorhanden sind, sofort mit der Rekonstruktion der Daten, allerdings kann RAID-5 auch nur den Verlust einer Partition verkraften. Genauso wie beim RAID-4 sind beim zeitgleichen Verlust zweier Partitionen alle Daten verloren. Eine /etc/raidtab für ein RAID-5 Device sähe folgendermaßen aus:
root@linux ~# raiddev /dev/md0
raid-level              5
nr-raid-disks           3
nr-spare-disks          2
persistent-superblock   1
parity-algorithm        left-symmetric
chunk-size              64
device                  /dev/sda6
raid-disk               0
device                  /dev/sdb6
raid-disk               1
device                  /dev/sdc6
raid-disk               2
device                  /dev/sdd6
spare-disk              0
device                  /dev/sde6
spare-disk              1

Bei diesem Beispiel sind gleich zwei Spare-Disks mit in die Konfiguration aufgenommen worden.

Der Parameter parity-algorithm legt die Art der Ablage der Paritätsinformationen fest und kann nur auf RAID-5 Verbunde angewendet werden. Die Auswahl left-symmetric bietet die maximale Performance. Weitere Möglichkeiten sind left-asymmetric, right-asymmetric und right-symmetric.

Initialisiert wird Ihr neues Device so:
root@linux ~# mkraid -f /dev/md0

Danach fehlt noch ein Dateisystem:
root@linux ~# mke2fs /dev/md0

Schon können Sie das Device überall hin mounten und in die /etc/fstab eintragen.

RAID-10 (Mirroring & Striping)

Die Kombination aus RAID-1 und RAID-0 Verbunden kann sehr flexibel eingesetzt werden, ist aber mit Vorsicht zu genießen. Man muss hierbei genau darauf achten, welche RAID-Partitionen in welchen RAID-Verbund eingebaut werden sollen. Um allerdings die nötige Redundanz gewährleisten zu können, sind hierfür mindestens vier RAID-Partitionen auf unterschiedlichen Festplatten nötig. Als Beispiel erstellen wir zwei RAID-0 Verbunde über jeweils zwei verschiedene RAID-Partitionen, die anschließend per RAID-1 gespiegelt werden sollen. Eine passende /etc/raidtab ohne Spare-Disks sähe dann so aus:
root@linux ~# raiddev /dev/md0
raid-level              0
nr-raid-disks           2
nr-spare-disks          0
persistent-superblock   1
chunk-size              4

device                  /dev/sda6
raid-disk               0
device                  /dev/sdb6
raid-disk               1
root@linux ~# raiddev /dev/md1
raid-level              0
nr-raid-disks           2
nr-spare-disks          0
persistent-superblock   1
chunk-size              4

device                  /dev/sdc6
raid-disk               0
device                  /dev/sdd6
raid-disk               1
root@linux ~# raiddev /dev/md2
raid-level              1
nr-raid-disks           2
nr-spare-disks          0
persistent-superblock   1
chunk-size              4

device                  /dev/md0
raid-disk               0
device                  /dev/md1
raid-disk               1

Jetzt gilt es aber ein paar Kleinigkeiten zu beachten, denn anders als bei den anderen RAID-Verbunden haben wir hier gleich drei RAID-Arrays erstellt, wobei man sich überlegen muss, welches denn nun nachher überhaupt gemountet und mit Daten beschrieben werden soll. Die Reihenfolge ergibt sich aus der Datei /etc/raidtab. /dev/md0 wird nachher auf /dev/md1 gespiegelt.

Jedes Devices muss für sich erstellt werden:
root@linux ~# mkraid -f /dev/mdx

Ein Dateisystem per mke2fs wird nur auf /dev/md0 und /dev/md1 erstellt.

Die beste Reihenfolge ist, erst das Device /dev/md0 zu erstellen, zu formatieren und zu mounten. Dann wird /dev/md1 erstellt und formatiert. Dieses bitte nicht mounten, da ja hier keine Daten drauf geschrieben werden sollen. Zuletzt wird nun mittels
root@linux ~# mkraid -f /dev/md2

das RAID-1 Array erstellt, jedoch sollte man hier wirklich kein Dateisystem erstellen. Ab jetzt kann man mittels
root@linux ~# cat /proc/mdstat

die Synchronisation der beiden RAID-0 Verbunde /dev/md0 und /dev/md1 verfolgen. Ist die Synchronisation abgeschlossen, werden alle Daten, die auf /dev/md0 geschrieben werden, auf /dev/md1 gespiegelt. Aktiviert, gemountet und in die Datei /etc/fstab eingetragen wird letzthin nur /dev/md0. Natürlich könnte man auch /dev/md1 mounten, jedoch sollte man sich für ein Device entscheiden. Ausgeschlossen ist allerdings das Mounten von /dev/md2. 	  

Weitere Optionen des neuen RAID-Patches
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Autodetection
"""""""""""""

Die Autodetection beschreibt einen Kernel-Parameter, der in allen beschriebenen Kernel-Vorbereitungen als zu aktivieren gekennzeichnet wurde. Er erlaubt das automatische Erkennen und Starten der diversen im System vorhandenen RAID-Verbunde schon während des Bootvorganges von Linux und somit auch die Nutzung eines RAID-Verbundes als Root-Partition.

Näheres zur Nutzung eines RAID-Verbundes als Root-Partition finden Sie im Abschnitt  Root-Partition oder Swap-Partition als RAID.

Persistent-Superblock
"""""""""""""""""""""

Diese überaus nützliche Option ist uns nun in jeder /etc/raidtab Konfiguration über den Weg gelaufen und mit dem Wert 1 eingetragen gewesen, doch was bedeutet er?

Erinnern Sie sich noch an die MD-Tools oder gar an die mdtab? Mit diesen älteren Tools wurde eine Datei /etc/mdtab erstellt, die in älteren RAID-Unterstützungen die Konfiguration Ihres RAID-Verbundes inklusiv einer Prüfsumme enthielt.

Sollte nun ein RAID-Verbund gestartet werden, so musste der Kernel erst mal diese Datei auslesen, um überhaupt zu erfahren, wo er welches RAID mit welchen Partitionen zu starten hatte. Haben Sie den Abschnitt über die Root-Partition als RAID gelesen, so ahnen Sie es schon: Um an diese Datei heranzukommen, muss aber erst mal das darrunterliegende Dateisystem laufen. Eine Zeit lang war es mit der neueren Konfigurationsdatei /etc/raidtab genauso, aber hier nun tritt der Parameter persistent-superblock in Aktion. Die möglichen Werte dafür sind 0 und 1. Ist der Wert auf 0 gesetzt, so verhält sich das Starten der RAIDs gemäß dem oben beschriebenen Vorgang. Ist er allerdings auf 1 gesetzt, wird beim Erstellen jedes neuen RAID-Verbundes an das Ende jeder Partition ein spezieller Superblock geschrieben, der es dem Kernel erlaubt, die benötigten Informationen über das RAID direkt von den jeweiligen Partitionen zu lesen, ohne ein Dateisystem gemountet zu haben. Trotzdem sollten Sie immer eine /etc/raidtab pflegen und beibehalten. Ist im Kernel die Autodetection aktiviert, so werden die RAID-Arrays mit aktiviertem Persistent-Superblock sogar direkt gestartet. Dies befähigt Sie, ganz simpel jedes RAID-Array als /dev/md0, /dev/md1 usw. einfach und problemlos in die /etc/fstab zu setzen. Der Kernel kümmert sich um das Aktivieren beim Startup, wodurch ein
root@linux ~# raidstart /dev/md0

ebenso wie ein
root@linux ~# raidstop /dev/md0

beim Systemhalt überflüssig ist.

Abgesehen davon ermöglicht diese Option auch das Booten von einem RAID-4 oder RAID-5 Array als Root-Partition. Näheres zur Einrichtung eines RAID-Arrays als Root-Partition finden Sie im Abschnitt  Root-Partition oder Swap-Partition als RAID.

Spare-Disks
"""""""""""

Spare-Disks bezeichnen Festplatten-Partitionen, die mit Hilfe der /etc/raidtab zwar schon einem bestimmten RAID-Verbund zugewiesen wurden, jedoch solange nicht benutzt werden, bis irgendwann mal eine Partition ausfällt. Dann allerdings wird die defekte Partition sofort durch die Spare-Disk ersetzt und die Rekonstruktion der Daten aus den Paritätsinformationen wird gestartet. Den Fortschritt dieser Rekonstruktion können Sie - wie alles über RAID-Devices - mittels
root@linux ~# cat /proc/mdstat

nachvollziehen. Eine Spare-Disk sollte mindestens genauso groß oder größer sein als die anderen verwendeten RAID-Partitionen. Ist eine Spare-Disk kleiner als die verwendeten RAID-Partitionen und ist der RAID-Verbund fast voll mit Daten, so kann bei einem Ausfall einer RAID-Partition die Spare-Disk natürlich nicht alle Daten aufnehmen, was unweigerlich zu Problemen führt.

Spare-Disks und Hot Plugging
""""""""""""""""""""""""""""

Dank der neuen RAID-Tools ist es auch möglich, eine Spare-Disk nachträglich in einen bereits vorhandenen RAID-Verbund einzufügen. Sinnvoll ist dieser Vorgang natürlich nur bei RAID-Modi, welche auch mit Spare-Disks umgehen können. Erweitern Sie dazu erst Ihre /etc/raidtab um die neue Spare-Disk. Dies ist zwar für den eigentlichen Vorgang nicht zwingend notwendig, jedoch empfiehlt es sich immer, eine sorgfältig gepflegte RAID-Konfigurationsdatei zu besitzen. Führen Sie dann zum Beispiel den Befehl
root@linux ~# raidhotadd /dev/md2 /dev/sde4

aus, um in Ihren dritten RAID-Verbund die vierte Partition Ihrer fünften SCSI-Festplatte einzufügen. Der Befehl raidhotadd fügt die neue Partition automatisch als Spare-Disk ein und aktualisiert auch gleich die Superblöcke aller in diesem RAID-Verbund befindlichen Partitionen. Somit brauchen Sie keine weiteren Schritte zu unternehmen, um die neue Spare-Disk Ihren anderen RAID-Partitionen als nutzbar bekannt zu geben.

Diesen Befehl kann man sich auch zu Nutze machen, einen intakten RAID-Verbund dazu zu bewegen, die Superblöcke neu zu schreiben oder sich zu synchronisieren. Der analog zu verwendende Befehl raidhotremove entfernt eine so hinzugefügte Spare-Disk natürlich auch wieder aus dem RAID-Array. 

Fehlerbehebung
^^^^^^^^^^^^^^

An dieser Stelle wird auf das Verhalten der unterschiedlichen RAID-Verbunde im Fehlerfall eingegangen. Die Szenarien zum Restaurieren defekter RAID-Verbunde reichen von den verschiedenen RAID-Modi bis hin zu Hot Plugging und Spare-Disks.

Die folgenden Beschreibungen stützen sich alle auf den Kernel 2.2.10 mit den passenden RAID-Tools und Kernel-Patches, entsprechen also der Einrichtfunktionalität des Abschnittes über die neuen RAID-Tools Version 0.9x. Die RAID-Verbunde, welche mit den MD-Tools unter der DLD 6.0 und DLD 6.01 eingerichtet und damit quasi auf die alte Art mit den RAID-Tools Version 0.4x erstellt wurden, werden mangels ausgiebiger Tests nicht berücksichtigt.

Da sich viele Möglichkeiten der Synchronisation und der überwachung von RAID-Verbunden auf spezielle Funktionen und Optionen der neuen RAID-Tools beziehen, kann man die im folgenden geschilderten Prozeduren nicht mal näherungsweise auf RAID-Verbunde, die mit den alten RAID-Tools Version 0.4x eingerichtet wurden, beziehen. Solange das noch nicht getestet wurde, sind Sie hier auf sich gestellt.

Linear (Append) Modus und RAID-0 (Stripping)
""""""""""""""""""""""""""""""""""""""""""""

Hier ist die Möglichkeit der Rekonstruktion von Daten schnell erklärt. Da diese RAID-Modi keine Redundanz ermöglichen, sind beim Ausfall einer Festplatte alle Daten verloren. Definitiv gilt das für RAID-0; beim Linear Modus können Sie eventuell mit viel Glück noch einige Daten einer RAID-Partition sichern, so es denn die erste Partition ist und Sie irgendwie in der Lage sind, die Partition einzeln zu Mounten. Da RAID-0 die Daten parallel schreibt, erhalten Sie - auch wenn Sie eine Partition des RAID-0 Verbundes gemountet bekommen - niemals mehr als einen zusammenhängenden Block. Meiner Meinung nach ist an dieser Stelle eine Datenrettung von einem RAID-0 Verbund sehr sehr schwierig.

RAID-1, RAID-4 und RAID-5 ohne Spare-Disk
"""""""""""""""""""""""""""""""""""""""""

Alle drei RAID-Modi versprechen Redundanz. Doch was muss gezielt getan werden, wenn hier eine Festplatte oder eine RAID-Partition ausfällt? Generell bieten sich einem zwei Wege, um die defekte Festplatte durch eine neue zu ersetzen. Bedenken Sie bei dieser Beschreibung, dass RAID-Partition und Festplatte synonym verwendet wird. Ich gehe von einer Partition je Festplatte aus. Die eine Methode beschreibt den heißen Weg. Hierbei wird die Festplatte im laufenden Betrieb ausgetauscht. Die andere Methode beschreibt entsprechend den sicheren Weg. Beide Möglichkeiten haben für die RAID-Modi 1, 4 und 5 funktioniert. Seien Sie insbesondere mit dem heißen Weg trotzdem sehr vorsichtig, da die Benutzung dieser Befehle nicht Hot Plugging fähige Hardware zerstören könnte.

Der "heiße" Weg (Hot Plugging) ohne Spare-Disk

Vorab muss gesagt werden, dass Hot Plugging unter Linux auch vom SCSI-Treiber unterstützt werden muss, versuchen Sie Hot Plugging aber niemals mit (E)IDE Laufwerken. Zwar sollen alle Linux SCSI-Treiber des hier verwendeten 2.2.10er Kernels Hot Plugging unterstützen, jedoch ist dies nur bei dem Adaptec und Symbios Treiber sicher der Fall. Auch sollte Ihre Hardware und damit Ihr SCSI Kontroller und Ihre SCSI-Festplatten Hot Plugging unterstützen. Wie man das herausbekommt, ist schwer zu sagen, allerdings hat es auch mit einem fünf Jahre alten Symbios Kontroller und mehreren IBM-DCAS-UW Festplatten funktioniert. Generell ist die meiste im Consumer Bereich verfügbare Hardware nicht Hot Plugging fähig. Die zu Testzwecken eingesetzten UW-Wechselrahmen kosten etwa 125,- EUR und haben alle Hot Plugging Tests klaglos verkraftet.

Haben Sie irgendwelche Zweifel und sind nicht gezwungen, Hot Plugging zu nutzen, dann lesen Sie hier gar nicht erst weiter, sondern springen Sie sofort zu der sicheren Beschreibung. Auch ergeben sich aus meinen erfolgreichen Tests keine Garantien für irgendeinen Erfolg. Mit allem Nachdruck muss daher an dieser Stelle auf die Möglichkeit der Zerstörung Ihrer Hardware durch die Benutzung von Hot Plugging mit ungeeigneter Hardware hingewiesen werden.

Ihr RAID-1, 4 oder 5 ist gemäß einer der vorherigen Anleitungen erstellt worden und betriebsbereit. Um den ganzen Ablauf zu vereinfachen, wird ab jetzt nur noch von einem RAID-5 ausgegangen und auch die Beispielauszüge mittels cat /proc/mdstat beschreiben ein RAID-5; RAID-1 und RAID-4 Benutzer können analog verfahren, richten aber bitte eine erhöhte Aufmerksamkeit auf die Unterscheidung und Abstraktion der Meldungsparameter.

Das Beispiel-RAID-5 ist in der /etc/raidtab so eingetragen:
root@linux ~# raiddev /dev/md0
raid-level              5
nr-raid-disks           4
nr-spare-disks          0
parity-algorithm        left-symmetric
persistent-superblock   1
chunk-size              32
device                  /dev/sda5
raid-disk               0
device                  /dev/sdb6
raid-disk               1
device                  /dev/sdc6
raid-disk               2
device                  /dev/sdd1
raid-disk               3

cat /proc/mdstat meldet einen ordentlich laufenden RAID-5-Verbund:
Personalities : [linear] [raid0] [raid1] [raid5] [hsm] read_ahead 1024 sectors
md0 : active raid5 sdd1[3] sdc6[2] sdb6[1] sda5[0] 633984 blocks level5, 32k chunk, algorithm 2 [4/4] [UUUU]
unused devices: <none>

Jetzt fällt die Festplatte /dev/sdd komplett aus. Beim nächsten Zugriff auf das RAID-5 wird der Mangel erkannt, der SCSI-Bus wird resetet und das RAID-5 läuft relativ unbeeindruckt weiter. Die defekte RAID-Partition wird lediglich mit (F) gekennzeichnet und fehlt in den durch ein U markierten gestarteten RAID-Partitionen:
Personalities : [linear] [raid0] [raid1] [raid5] [hsm] read_ahead 1024sectors
md0 : active raid5 sdd1[3](F) sdc6[2] sdb6[1] sda5[0] 633984 blocks level 5, 32k chunk, algorithm 2 [4/3] [UUU_]
unused devices: <none>

Jetzt möchten Sie wieder ein redundantes System bekommen, ohne den Rechner neu booten zu müssen und ohne Ihr gemountetes RAID-5 stoppen zu müssen. Entfernen Sie dafür die defekte RAID-Partition (/dev/sdd1) mittels des bei den RAID-Tools beiliegenden Hilfsprogramms raidhotremove aus dem aktiven RAID-Verbund (/dev/md0):
root@linux ~# raidhotremove /dev/md0 /dev/sdd1

Ein cat /proc/mdstat hat sich nun dahingehend verändert, dass die defekte Partition komplett rausgenommen wurde und Ihnen eine fehlende Partition, um Redundanz zu gewährleisten, mit [UUU_] angezeigt wird:
Personalities : [linear] [raid0] [raid1] [raid5] [hsm] read_ahead 1024sectors
md0 : active raid5 sdc6[2] sdb6[1] sda5[0] 633984 blocks level 5, 32k chunk, algorithm 2 [4/3] [UUU_]
unused devices: <none>

Tauschen Sie jetzt Ihre hoffentlich in einem guten Wechselrahmen befindliche defekte Festplatte gegen eine neue aus und erstellen Sie eine Partition darauf. Anschließend geben Sie den Befehl, diese neue RAID-Partition wieder in das laufende Array einzubinden:
root@linux ~# raidhotadd /dev/md0 /dev/sdd1

Der Daemon raid5d macht sich nun daran, die neue RAID-Partition mit den anderen zu synchronisieren:
Personalities : [linear] [raid0] [raid1] [raid5] [hsm] read_ahead 1024sectors
md0 : active raid5 sdd1[4] sdc6[2] sdb6[1] sda5[0] 633984 blockslevel5, 32k chunk, algorithm 2 [4/3] [UUU_] recovery=7% finish=4.3min
unused devices: <none>

Der Abschluss dieses Vorganges wird mit einem ebenso lapidaren wie beruhigendem resync finished quittiert. Eine Kontrolle ergibt tatsächlich ein wieder vollständig redundantes und funktionstüchtiges RAID-5:
Personalities : [linear] [raid0] [raid1] [raid5] [hsm] read_ahead 1024sectors
md0 : active raid5 sdd1[3] sdc6[2] sdb6[1] sda5[0] 633984 blocks level5, 32k chunk, algorithm 2 [4/4] [UUUU]
unused devices: <none>

Als ob nicht gewesen wäre.

Der sichere Weg ohne Spare-Disk

Diese Methode sieht einen Wechsel einer defekten Festplatte in einem System vor, das ruhig für einige Zeit heruntergefahren werden kann.

Ihr RAID-1, 4 oder 5 ist gemäß einer der vorherigen Anleitungen erstellt worden und betriebsbereit. Um den ganzen Ablauf zu vereinfachen, wird ab jetzt nur noch von einem RAID-5 ausgegangen und auch die Beispielauszüge mittels cat /proc/mdstat beschreiben ein RAID-5; RAID-1 und RAID-4 Benutzer können analog verfahren, richten aber bitte eine erhöhte Aufmerksamkeit auf die Unterscheidung und Abstraktion der Meldungsparameter.

Das Beispiel RAID-5 ist in der /etc/raidtab so eingetragen:
root@linux ~# raiddev /dev/md0
raid-level              5
nr-raid-disks           4
nr-spare-disks          0
parity-algorithm        left-symmetric
persistent-superblock   1
chunk-size              32
device                  /dev/sda5
raid-disk               0
device                  /dev/sdb6
raid-disk               1
device                  /dev/sdc6
raid-disk               2
device                  /dev/sdd1
raid-disk               3

cat /proc/mdstat meldet einen ordentlich laufenden RAID-5-Verbund:
Personalities : [linear] [raid0] [raid1] [raid5] [hsm]
read_ahead 1024 sectors
md0 : active raid5 sdd1[3] sdc6[2] sdb6[1] sda5[0] 633984 blocks level5, 32k chunk, algorithm 2 [4/4] [UUUU]
unused devices: <none>

Jetzt fällt die Festplatte /dev/sdd komplett aus. Beim nächsten Zugriff auf das RAID-5 wird der Mangel erkannt, der SCSI-Bus wird resetet und das RAID-5 läuft relativ unbeeindruckt weiter. Die defekte RAID-Partition wird lediglich mit (F) gekennzeichnet:
Personalities : [linear] [raid0] [raid1] [raid5] [hsm] read_ahead 1024sectors
md0 : active raid5 sdd1[3](F) sdc6[2] sdb6[1] sda5[0] 633984 blocks level 5, 32k chunk, algorithm 2 [4/3] [UUU_]
unused devices: <none>

Jetzt möchten Sie wieder ein redundantes System bekommen. Fahren Sie dazu Ihren Rechner herunter und tauschen Sie die defekte Festplatte gegen eine neue aus. Nach dem Startup müssen Sie auf der neuen Festplatte eine entsprechende Partition möglichst auch mit der RAID-Autostart Option durch das fd Flag einrichten. Ist die neue Partitionsangabe identisch mit der auf der defekten Festplatte, brauchen Sie nichts weiter zu machen, ansonsten müssen Sie noch Ihre /etc/raidtab anpassen. Weiterhin ist es erforderlich, dass Ihr RAID-Verbund läuft. Ist das nicht bereits während des Bootvorganges erfolgt, müssen Sie selbiges jetzt nachholen:
root@linux ~# raidstart /dev/md0

Jetzt fehlt noch der Befehl, um die neue RAID-Partition wieder in das RAID-5 einzuarbeiten und die persistent-superblocks neu zu schreiben. Hierbei werden keine vorhandenen Daten auf dem bestehenden RAID-5 Verbund zerstört, es sei denn, Sie haben sich bei der eventuell nötigen Aktualisierung der /etc/raidtab vertan. Prüfen Sie alles dringend noch mal. Ansonsten:
root@linux ~# raidhotadd /dev/md0 /dev/sdd1

Dieser Befehl suggeriert zwar, dass hier eine Art Hot Plugging stattfindet, heißt in diesem Zusammenhang aber nichts anderes, als dass eine RAID-Partition in ein vorhandenes RAID-Array eingearbeitet wird. Würden Sie stattdessen den Befehl mkraid mit seinen Parametern aufrufen, würde dies zwar auch zu dem Erfolg führen, ein neues RAID-Array zu erstellen, jedoch dummerweise ohne Daten und damit natürlich auch und vor allem ohne die bisher vorhandenen Daten.

Inspizieren Sie anschließend den Verlauf wieder mittels cat /proc/mdstat:
Personalities : [linear] [raid0] [raid1] [raid5] [hsm] read_ahead 1024sectors
md0 : active raid5 sdd1[3] sdc6[2] sdb6[1] sda5[0] 633984 blocks level5, 32k chunk, algorithm 2 [4/4] [UUUU] resync=57% finish=1.7min
unused devices: <none>

Der Abschluss dieses Vorganges wird mit einem ebenso lapidaren wie beruhigendem resync finished quittiert. Eine Kontrolle ergibt tatsächlich ein wieder vollständig redundantes und funktionstüchtiges RAID-5:
Personalities : [linear] [raid0] [raid1] [raid5] [hsm] read_ahead 1024sectors
md0 : active raid5 sdd1[3] sdc6[2] sdb6[1] sda5[0] 633984 blocks level5, 32k chunk, algorithm 2 [4/4] [UUUU]
unused devices: <none>

Das RAID-5 Array muss nun lediglich wieder in den Verzeichnisbaum gemountet werden, falls Ihr RAID-Array nicht bereits innerhalb der /etc/fstab während des Bootvorganges gemountet wurde:
root@linux ~# mount /dev/md0 /mount-point

Hiermit haben Sie wieder ein vollständig redundantes und lauffähiges RAID-5 Array hergestellt.

RAID-1, RAID-4 und RAID-5 mit Spare-Disk
""""""""""""""""""""""""""""""""""""""""

Der "heiße" Weg (Hot Plugging) mit Spare-Disk

Um bei einem RAID-1, 4 oder 5 Verbund mit einer Spare-Disk per Hot Plugging, also ohne den RAID-Verbund auch nur herunterzufahren, eine RAID-Partition per raidhotremove aus dem laufenden Verbund zu entfernen und durch eine neue RAID-Partition zu ersetzen, sind die aktuellsten RAID-Tools erforderlich. Erst diese haben durch das neue Programm raidsetfaulty die Möglichkeit, die ausgefallene RAID-Partition als defekt zu markieren und so den Befehl raidhotremove zu ermöglichen. Zu beachten ist hier, dass bei einem Festplattenausfall die Spare-Disk sofort eingearbeitet und das System anschließend auch wieder Redundant ist und somit natürlich nicht die Spare-Disk, sondern die ausgefallene RAID-Partition als defekt markiert, ausgetauscht und als neue Spare-Disk wieder eingesetzt werden muss.

Auch an dieser Stelle muss auf die Gefahr der teilweisen oder vollständigen Zerstörung Ihrer Hardware hingewiesen werden, sollte diese nicht Hot Plugging fähig sein. Haben Sie die Möglichkeit zu wählen, benutzen Sie immer die sichere Methode.

Der sichere Weg mit Spare-Disk

Ein normal laufender RAID-5 Verbund mit Spare-Disk sollte bei einem RAID-Verbund /dev/md0 mit den RAID-Partitionen /dev/sdb1, /dev/sdc1 und /dev/sdd1 plus der Spare-Disk /dev/sde1 unter /proc/mdstat folgendes zeigen:
Personalities : [raid5]read_ahead 1024 sectors
md0 : active raid5 sde1[3] sdd1[2] sdc1[1] sdb1[0] 782080 blocks level5, 32k chunk, algorithm 2 [3/3] [UUU]
unused devices: <none>

Durch das Entriegeln des Wechselrahmens wird ein Defekt der Partition /dev/sdc1 simuliert. Sobald wieder auf den RAID-5 Verbund zugegriffen wird, wird der Defekt bemerkt, der SCSI-Bus resetet und der Recovery-Prozess über den raid5d Daemon beginnt. Ein cat /proc/mdstat zeigt jetzt folgendes:
Personalities : [raid5] read_ahead 1024 sectors
md0 : active raid5 sde1[3] sdd1[2] sdc1[1](F) sdb1[0] 782080 blocks level 5, 32k chunk, algorithm 2 [3/2] [U_U] recovery=4% finish=15.4min
unused devices: <none>

Nach dem erfolgreichen Ende des Recovery-Prozesses liefert cat /proc/mdstat folgendes:
Personalities : [raid5] read_ahead 1024 sectors
md0 : active raid5 sde1[3] sdd1[2] sdc1[1](F) sdb1[0] 782080 blocks level 5, 32k chunk, algorithm 2 [3/2] [U_U]
unused devices: <none>

Den neuen Zustand sollten Sie nun sichern, indem Sie
root@linux ~# umount /dev/md0

ausführen. Mit
root@linux ~# raidstop /dev/md0

wird der aktuelle Zustand auf die RAID-Partitionen geschrieben. Ein
root@linux ~# raidstart -a
root@linux ~# cat /proc/mdstat

zeigt dann:
Personalities : [raid5] read_ahead 1024 sectors
md0 : active raid5 sdd1[2] sde1[1] sdb1[0] 782080 blocks level 5, 32k chunk, algorithm 2 [3/3] [UUU]
unused devices: <none>

Wie Sie erkennen können, fehlt die defekte Partition /dev/sdc1[1](F), dafür hat die Spare-Disk /dev/sde1[1] deren Funktion übernommen. Das ist jetzt der aktuelle Zustand des RAID-5. Nun wird die defekte Festplatte ersetzt, indem Sie den Rechner herunterfahren und den Festplattenaustausch durchführt. Wenn Sie neu booten, geht zunächst nichts mehr. Nun bloß keine Panik kriegen, sondern erst mal der /etc/raidtab Datei den neuen Zustand beibringen:
root@linux # raiddev /dev/md0
raid-level              5
nr-raid-disks           3
nr-spare-disks          1
persistent-superblock   1
parity-algorithm        left-symmetric
chunk-size              32
device                  /dev/sdb1
raid-disk               0
device                  /dev/sde1
raid-disk               1
device                  /dev/sdd1
raid-disk               2
device                  /dev/sdc1
spare-disk              0

Anschließend bringt ein
root@linux ~# raidhotadd /dev/md0 /dev/sdc1

dem RAID-Verbund die neue Konstellation bei, ohne dabei die Daten auf dem RAID-5 Verbund zu beschädigen, solange Sie sich in der Datei /etc/raidtab nicht vertan haben. Schauen Sie sich die Einträge in Ihrem eigenen Interesse bitte noch mal genau an. Ein cat /proc/mdstat zeigt jetzt die Resynchronisation. Man sieht jetzt die neue Zuordnung der RAID-Partitionen im RAID-Verbund, die exakt den neuen Stand der Zuordnung darstellen sollte.
Personalities : [raid5] read_ahead 1024 sectors
md0 : active raid5 sdc1[3] sdd1[2] sde1[19] sdb1[0] 782080 blocks level 5, 32k chunk, algorithm 2 [3/3] [UUU] resync=36% finish=6.7min
unused devices: <none>

Am Ende erscheint:
raid5: resync finished

Ein cat /proc/mdstat sieht nun so aus:
Personalities : [raid5] read_ahead 1024 sectors
md0 : active raid5 sdc1[3] sdd1[2] sde1[1] sdb1[0] 782080 blocks level5, 32k chunk, algorithm 2 [3/3] [UUU]
unused devices: <none>

Nun ruft man
root@linux ~# raidstop /dev/md0

auf, um alles auf die Platten zu schreiben. Hat der Kernel das RAID-Array bereits gestartet (persistent-superblock 1), kann man mit einem
root@linux ~# mount /dev/md0 /mount-point

den RAID-Verbund wieder in das Dateisystem einhängen. Ansonsten ist vorher noch folgender Befehl notwendig:
root@linux ~# raidstart /dev/md0

Hiermit haben Sie wieder ein vollständiges laufendes RAID-5 Array hergestellt.

Nutzung & Benchmarks
^^^^^^^^^^^^^^^^^^^^

Wofür lohnt das Ganze denn nun?
"""""""""""""""""""""""""""""""

Performance

Ein RAID-Device lohnt sich überall dort, wo viel auf die Festplatten zugegriffen wird. So kann zum Beispiel ein RAID-0 oder RAID-5 als /home Verzeichnis gemountet werden und das Nächste als /var oder /usr. Die Geschwindigkeitsvorteile sind gerade bei SCSI Hardware und festplattenintensiven Softwarepaketen wie KDE, StarOffice oder Netscape deutlich spürbar. Das ganze System fühlt sich erheblich performanter an.

Sicherheit

Mit einem RAID-1 System kann man z.B. die Sicherheit seiner Daten erhöhen. Fällt eine Platte aus, befinden sich die Daten immer noch auf der gespiegelten Partition. ähnliches gilt z.B. für ein RAID-5 System, welches zusätzlich zur erhöhten Sicherheit noch eine bessere Performance bietet.

Warum nicht?

Hat man schon mehrere Festplatten in seinem System, stellt sich einem doch die Frage, warum man eigentlich solch ein kostenloses Feature wie Software-RAID nicht nutzen sollte. Man denke nur mal an die Preise für gute Hardware-RAID Kontroller plus Speicher. Gerade der neue Kernel-Patch erleichtert einem vieles, was bisher nur auf Umwegen möglich war.

Schreitet die Entwicklung der Software-RAID Unterstützung unter Linux weiterhin so gut fort und bedenkt man die stetig steigende Leistung und die fallenden Preise von CPUs, so kann man sich denken, dass in naher Zukunft die CPU-Leistung, die eine Software-RAID Lösung benötigt, auch bei Standard-CPUs nicht mehr ins Gewicht fällt. Selbst heute reichen für ein Software-RAID auf SCSI-Basis CPUs mit 200-300 MHz völlig aus. Ein RAID-0 mit SCSI-Festplatten soll sogar auf einem 486DX-2/66 halbwegs akzeptabel laufen.

Vorschläge und überlegungen zur RAID-Nutzung
""""""""""""""""""""""""""""""""""""""""""""

Apache (Webserver allgemein)

Aufgrund des hauptsächlichen Lesevorgangs kann hier ein RAID-0 oder RAID-5 Verbund empfohlen werden. Die Log-Dateien sollten allerdings nicht auf einem RAID-5 Verbund liegen. Werden durch dynamische Seiten Daten mit einer Datenbank ausgetauscht, gelten dieselben überlegungen wie bei dem Datenbankabschnitt.

Log-Dateien

Die typischen Sytem-Log-Dateien erbringen im allgemeinen keine hohe Dauerschreiblast. Hierbei ist es also egal, auf was für einem RAID-Verbund sie liegen. Anders sieht es bei Log-Dateien aus, welche viele Accounting-Informationen enthalten. Diese sollte man aufgrund der allgemeinen Schreibschwäche eines RAID-5 Verbundes eher auf schnellere RAID-Verbunde auslagern.

Oracle (Datenbank)

Oracle kann mit Tablespaces, die aus mehreren Datenfiles auf unterschiedlichen Platten bestehen, umgehen. Um hier zu einer vernünftigen Lastverteilung ohne RAID zu kommen, ist allerdings einiges an Planung vorauszusetzen. Für die Datenfiles und Controlfiles können RAID-0, RAID-1, RAID-10 oder auch RAID-5 Verbunde empfohlen werden; aus Sicherheitsgründen ist allerdings von RAID-0 Verbunden abzuraten. Für die Online-Redo-Log-Dateien empfiehlt sich aufgrund der Schreibschwächen kein RAID-5 System. Dafür könnten folgende Varianten konfiguriert werden:

    einfache Redo-Log-Dateien auf einem RAID-1 oder RAID-10 Verbund
    gespiegelte Redo-Log-Dateien jeweils auf einem RAID-0 Verbund; Oracle kann so konfiguriert werden, dass mehrere parallele Kopien der Redo-Log-Dateien geschrieben werden.


Squid (WWW-Proxy und Cache)

Für Squid im speziellen gilt, dass er von Haus aus mit mehreren Cache-Partitionen umgehen kann. Daher bringt ein darunterliegendes RAID-Array nicht mehr viel. Die Log-Dateien wiederum ergeben auf einem RAID-0 Verbund durchaus Sinn. Für andere WWW-Proxys wäre hier ein RAID-0 Verbund angebracht.

Systemverzeichnisse

Da hier verhältnismäßig wenig Schreibvorgänge stattfinden, jedoch auf jeden Fall die Redundanz gewährleistet sein sollte, empfiehlt sich für die Root-Partition ein RAID-1 Verbund und für die /home, /usr, /var Verzeichnisse einer der redundanten RAID-Modi (RAID-1, RAID-5, RAID-10).

Benchmarks
""""""""""

Um einen Vergleich zwischen den RAID-Verbunden mit ihren unterschiedlichen Chunk-Size Parametern ziehen zu können, findet das Programm Bonnie Anwendung, welches im Internet unter

    en http://www.textuality.com/bonnie/ 

residiert. Bonnie erstellt eine beliebig große Datei auf dem RAID-Verbund und testet neben den unterschiedlichen Schreib- und Lesestrategien auch die anfallende CPU-Last. Allerdings sollte man die Testdatei mindestens doppelt so groß wie den real im Rechner vorhandenen RAM-Speicher wählen, da Bonnie sonst nicht die Geschwindigkeit des RAIDs, sondern das Cacheverhalten von Linux misst - und das ist gar nicht mal so schlecht.

Um die bei dem nicht sequentiellen Lesen von einem RAID-1 Verbund höhere Geschwindigkeit zu testen, eignet sich Bonnie aufgrund seiner einzelnen Testdatei allerdings nicht. Bonnie führt bei seinem Test also einen sequentiellen Schreib-/Lesevorgang durch, welcher nur von einer RAID-1 Festplatte beantwortet wird. Möchte man trotzdem einen RAID-1 Verbund geeignet testen, empfiehlt sich hierfür das Programm tiotest welches unter

    en http://www.iki.fi/miku/ 

zu finden ist.

Da die Hardware wohl in jedem Rechner unterschiedlich ist, wurde darauf verzichtet, in der Ergebnistabelle absolute Werte einzutragen. Ein besserer Vergleich ergibt sich, wenn man die Geschwindigkeit einer Festplatte alleine als den Faktor eins zugrundelegt und bei Verwendung derselben Festplatten die RAID-Performance als Vielfaches dieses Wertes angibt. Da diese Konfiguration drei identische SCSI-Festplatten vorsieht, wäre also maximal eine 3x1fache Geschwindigkeit zu erwarten. Bedenken sollte man beim RAID-5 Testlauf auch, dass hier nur 2/3 der Kapazität direkt von Bonnie beschrieben werden. Andererseits soll dieser Test ja zeigen, wie viel Geschwindigkeitseinbußen oder -gewinn bei einer Datei derselben Größe zu erwarten sind.
  |            |              |                   |                  |                  |
|RAID-Modus  |  Chunk-Size  |  Blockgröße ext2  |  Seq. Input      |  Seq. Output     |
|            |              |                   |                  |                  |
|Normal      |  %           |  4 KB             |  1 = Referenz    |  1 = Referenz    |
|RAID-0      |  4 KB        |  4 KB             |  2,6 x Referenz  |  2,8 x Referenz  |
|RAID-5      |  32 KB       |  4 KB             |  1,7 x Referenz  |  1,9 x Referenz  |
|RAID 10     |  4 KB        |  4 KB             |  1,7 x Referenz  |  2,5 x Referenz  |

Wie zu erwarten erreicht ein RAID-0 Verbund aus drei identischen Festplatten annähernd die dreifache Leistung. Auch die RAID-5 Ergebnisse zeigen die bei drei Festplatten erwartete doppelte Leistung. Die Geschwindigkeit der dritten Festplatte kann ja aufgrund der Paritätsinformation nicht gemessen werden. Die Leistung eines RAID-5 sollte also allgemein der aller verwendeten Platten minus eins für die Paritätsinformationsplatte sein. Allerdings leidet RAID-5 an einer Art chronischer Schreibschwäche, welche durch das Berechnen und Ablegen der Paritätsinformationen zu erklären ist.

Um sich die Belastung des Prozessors und die benötigte Zeit zum Schreiben einer Testdatei anzusehen, kann man sich auch wieder des Programms dd befleißigen. Der folgende Aufruf von dd würde eine 100 MB große Datei in das aktuelle Verzeichnis schreiben und die Ergebnisse anzeigen:
root@linux ~# time dd if=/dev/zero of=./Testdatei bs=1024 count=102400 

Tipps und Tricks
^^^^^^^^^^^^^^^^

Hier finden sowohl Tipps und Tricks Erwähnung, die teilweise selbst getestet wurden, als auch Besonderheiten, die nicht direkt durch die RAID-Kernelerweiterungen ermöglicht werden, sondern allgemeiner Natur sind oder zusätzlich integriert werden müssen. Einige der hier aufgelisteten Vorgänge sind mit allerhöchster Vorsicht zu genießen und mehr oder minder ausdrücklich nicht für eine Produktionsumgebung geeignet.

DRBD
""""

Drbd versteht sich als ein Block Device, um eine Hochverfügbarkeitslösung unter Linux zu bieten. Im Prinzip ist es eine Art RAID-1 Verbund, der über ein Netzwerk läuft. Nähere Informationen hierzu gibt es unter:

    en http://www.complang.tuwien.ac.at/reisner/drbd/ 

Kernel 2.2.11 bis 2.2.13 und der RAID-Patch
"""""""""""""""""""""""""""""""""""""""""""

Um Linux Software-RAID auch mit dem aktuelleren Kerneln zu verwenden, kann man einfach den RAID-Patch für den 2.2.10er Kernel auf den Sourcetree der 2.2.11er bis 2.2.13er Kernel anwenden. Zweimal kommt vom patch die Frage, ob eine bereits gepatchte Datei noch mal gepatcht werden soll. Diese Fragen sollten alle mit "no" beantwortet werden. Der Erfolg ist, dass der so gepatchte Kernel nach dem Kompilierlauf hervorragend mit allen RAID-Modi funktioniert. Wer sich den 2.2.10er RAID-Patch ersparen möchte, kann sich einen Kernel-Patch von Alan Cox für den 2.2.11er bis 2.2.13er Kernel aus dem Internet besorgen:

    ftp://ftp.kernel.org:/pub/linux/kernel/people/alan/ 

Diese enthalten auch die jeweils aktuellen RAID-Treiber.

Kernel 2.2.14 bis 2.2.16 und der RAID-Patch
"""""""""""""""""""""""""""""""""""""""""""

Auch in den aktuellsten 2.2.xer Linux Kerneln ist der aktuelle RAID-Patch noch nicht enthalten. Passende Patches hierfür findet man im Internet bei Ingo Molnar:

    en http://people.redhat.com/mingo/raid-patches/ 


Kernel der 2.4.xer Reihe und der RAID-Patch
"""""""""""""""""""""""""""""""""""""""""""

Alle aktuellen Kernel der 2.4.xer Reihe beinhalten bereits die neue RAID-Unterstützung. Diese Kernel müssen nicht mehr gepatcht werden, lassen sich mit den aktivierten RAID-Optionen einwandfrei kompilieren und funktionieren problemlos.

SCSI-Festplatte zum Ausfallen bewegen
"""""""""""""""""""""""""""""""""""""

Für SCSI Devices aller Art - also nicht nur Festplatten - gibt es unter Linux die Möglichkeit, sie während des laufenden Betriebes quasi vom SCSI Bus abzuklemmen; dies allerdings ohne Hand an den Stromstecker oder Wechselrahmenschlüssel legen zu müssen. Möglich ist dies durch den Befehl:
root@linux # echo "scsi remove-single-device c b t l" > /proc/scsi/scsi

Die Optionen stehen für:

    c = die Nummer des SCSI-Controllers
    b = die Nummer des Busses oder Kanals
    t = die SCSI ID
    l = die SCSI LUN 

Möchte man das 5. Device des 1. SCSI-Controllers rausschmeißen, müsste der Befehl so aussehen:
root@linux # echo "scsi remove-single-device 0 0 5 0" > /proc/scsi/scsi

Umgekehrt funktioniert das Hinzufügen einer so verbannten Festplatte natürlich auch:
root@linux # echo "scsi add-single-device c b t l" > /proc/scsi/scsi

überwachen der RAID-Aktivitäten
"""""""""""""""""""""""""""""""

Um einen überblick über den Zustand des RAID-Systems zu bekommen, gibt es mehrere Möglichkeiten:

mdstat

    Mit einem 

root@linux ~# cat /proc/mdstat

    haben wir uns auch bisher immer einen überblick über den aktuellen Zustand des RAID-Systems verschafft. 

vmstat

    Ist man an der fortlaufenden CPU und Speicher-Belastung sowie an der Festplatten I/O-Belastung interessiert, sollte man auf der Konsole einfach ein 

root@linux ~# vmstat 1

    ausprobieren. 

Mit grep oder perl mdstat abfragen

    Wie üblich lässt sich unter Linux auch eine Abfrage von /proc/mdstat mit einem Skript realisieren. Hier könnte man z.B. die Folge "[UUUU]" nach einem Unterstrich regelmäßig per cron abfragen - der Unterstrich signalisiert ja eine defekte RAID-Partition - und lässt sich diese Information dann als E-Mail schicken. Lässt man sich allerdings schon eine E-Mail schicken, kann man sich ebenso gut auch eine SMS an sein Handy schicken lassen. Dieser Weg ist dann auch nicht mehr weit. Ein passendes grep-Kommando zum Abfragen von /proc/mdstat könnte dieses Aussehen haben: 

root@linux ~# grep '[\[U]_' /proc/mdstat

xosview

    Die aktuelle Entwicklerversion dieses bekannten überwachungsprogramms enthält bereits eine Statusabfrage für RAID-1 und RAID-5 Verbunde. Für den ordnungsgemäßen Betrieb ist ein weiterer Kernel-Patch notwendig, der jedoch im tar-Archiv enthalten ist. 


Verändern des read-ahead-Puffers
""""""""""""""""""""""""""""""""

Um den read-ahead-Puffer jeglicher Major-Devices unter Linux einfach ändern zu können, gibt es ein nettes kleines Programm:
read-ahead-Puffer

  /* readahead -- set & get the read_ahead value for the specified device
  */

  #include "stdio.h"
  #include "stdlib.h"
  #include "linux/fs.h"
  #include "asm/fcntl.h"

  void usage()
  {
    printf( "usage:  readahead <device> [newvalue]\n" );

  }/* usage() */


  int main( int args, char **argv )
  {
    int fd;
    int oldvalue;
    int newvalue;

    if ( args <= 1 ) {
      usage();
      return(1);
    }

    if ( args >= 3 && !isdigit(argv[2][0]) ) {
      printf( "readahead: invalid number.\n" );
      return(1);
    }

    fd = open( argv[1], O_RDONLY );
    if ( fd == -1 ) {
      printf( "readahead: unable to open device %s\n", argv[1] );
      return(1);
    }

    if ( ioctl(fd, BLKRAGET, &oldvalue) ) {
      printf( "readahead: unable to get read_ahead value from "
              "device %s\n", argv[1] );
      close (fd);
      return(1);
    }

    if ( args >= 3 ) {
      newvalue = atoi( argv[2] );
      if ( ioctl(fd, BLKRASET, newvalue) ) {
        printf( "readahead: unable to set %s's read_ahead to %d\n",
                argv[1], newvalue );
        close (fd);
        return(1);
      }
    }
    else {
      printf( "%d\n", oldvalue );
    }

    close (fd);
    return(0);

  }/* main */
	  
	 

Damit kann man natürlich auch RAID-Devices tunen.

Bestehenden RAID-0 Verbund erweitern
""""""""""""""""""""""""""""""""""""

Um einen RAID-0 Verbund zu vergrößern, zu verkleinern oder aus einer einzelnen Festplatte ein RAID-0 zu erstellen, gibt es von Jakob Oestergaard einen Patch für die RAID-Tools:

    http://ostenfeld.dk/~jakob/raidreconf/raidreconf-0.0.2.patch.gz 

Dieser Patch muss in den Source der RAID-Tools eingearbeitet werden und die RAID-Tools anschließend neu kompiliert und installiert werden. Das Erweitern eines RAID-0 Verbundes funktioniert dann durch zwei unterschiedliche /etc/raidtab Dateien, die miteinander verglichen werden und eine zusätzliche Partition innerhalb desselben Verbundes eingearbeitet wird. Nach dem stoppen des zu verändernden RAID-Verbundes, erfolgt der Aufruf durch:
root@linux ~# raidreconf /etc/raidtab /etc/raidtab.neu /dev/md0

Hierbei muss in der /etc/raidtab.neu für den Verbund /dev/md0 eine weitere Partition im Gegensatz zu /etc/raidtab eingetragen sein.

Bestehenden RAID-5 Verbund erweitern
""""""""""""""""""""""""""""""""""""

Einen bereits initialisierten und laufenden RAID-5 Verbund kann man derzeit leider nicht mit weiteren Festplatten vergrößern. Die einzige Möglichkeit besteht darin, die Daten zu sichern, den RAID-5 Verbund neu aufzusetzen und die Daten anschließend zurück zu schreiben.

Lizenz

    GPL

Autor

    Niels Happel  happel@resultings.de

	
Formatierung

    Matthias Hagedorn matthias.hagedorn@selflinux.org


Linux LVM-HOWTO
---------------

Beschreibung

Dieses HOWTO beschreibt die Nutzung der LVM-Funktion, die seit dem Standard-Kernel 2.4 in Linux implementiert ist.

Grundlagen
^^^^^^^^^^

Was ist LVM?
""""""""""""

LVM ist die Abkürzung für "Logical Volume Manager" und bezeichnet eine Funktion, die seit der Version 2.4 im Standard-Kernel integriert ist. Für Windows-Anhänger entspricht dies in etwa den "Dynamischen Datenträgern" bei Microsoft Windows 2000 oder XP Pro. Mittels LVM lässt sich eine logische Schicht zwischen Dateisystem und der Partition einer physikalischen Festplatte schieben. So ist es möglich, ein Dateisystem über mehrere Partitionen und Festplatten zu strecken, wohlgemerkt auch nach dem Anlegen eines Dateisystems, sogar wenn schon Daten darin abgespeichert wurden. Dazu wird das Dateisystem auf einer virtuellen Partition, einem so genannten Logical Volume, angelegt. Dies ist auch der eigentliche Clou von LVM: Man kann einer zu kleinen Partition oder Festplatte, die mit LVM verwaltet wird, nachträglich freien Speicherplatz hinzufügen. Voraussetzung ist allerdings, dass die betreffenden Partitionen im Voraus von LVM verwaltet wurden. LVM kann nicht auf bestehende Datenpartitionen angewandt werden. Eine zusätzliche und praktische Eigenschaft von LVM ist dessen Backup-Funktion. Mittels so genannten Snapshots kann man sehr einfach eine identische Kopie seiner von LVM verwalteten Partition erstellen. Ein weiterer Vorteil besteht darin, dass LVM die Geschwindigkeit bei Schreib- und Lesezugriffen nicht merklich beeinträchtigt, und auch die CPU-Belastung steigt kaum. Jedoch steigt analog zu RAID 0 die Ausfallwahrscheinlichkeit, wenn sich das Dateisystem und die darunter befindliche virtuelle Partition über mehrere Festplatten erstreckt, da nur eine Festplatte ausfallen muss, um die ganzen Daten zu verlieren.

Warnung 
"""""""

Der Autor übernimmt keine Garantie auf die hier beschriebenen Verfahren und übernimmt keine Haftung für eventuelle Hardwareschäden und/oder Datenverluste oder sonstige Schäden. LVM ist zwar inzwischen sehr stabil und ausgereift, dennoch sollte man Arbeiten am Dateisystem nie ohne Backup vornehmen. Sichern Sie bevor Sie fortfahren Ihre Daten auf einen gesonderten Datenträger, um Datenverluste zu vermeiden.

Copyright
"""""""""

Dieses Dokument ist urheberrechtlich geschützt. Das Copyright für dieses Dokument liegt bei Markus Hoffmann (  mar.hoff@gmx.net).

Das Dokument darf gemäß der  GNU General Public License verbreitet werden. Insbesondere bedeutet dieses, dass der Text sowohl über elektronische wie auch physikalische Medien ohne die Zahlung von Lizenzgebühren verbreitet werden darf, solange dieser Copyright-Hinweis nicht entfernt wird. Eine kommerzielle Verbreitung ist erlaubt und ausdrücklich erwünscht. Bei einer Publikation in Papierform ist das de Deutsche Linux HOWTO Projekt hierüber zu informieren.


Voraussetzungen
^^^^^^^^^^^^^^^

Um LVM nutzen zu können müssen Sie feststellen, ob Ihr Kernel diese Funktion unterstützt, was gewöhnlich ab dem Standard-Kernel 2.4 der Fall ist. Dazu dient das Modul lvm-mod. Führen Sie als Benutzer root den Befehl lsmod aus, um zu überprüfen, ob es in der Liste der geladenen Module schon enthalten ist, gegebenenfalls führen Sie vorher noch modprobe lvm-mod aus. Zusätzlich dazu können Sie auch überprüfen, ob das Verzeichnis /proc/lvm existiert, welches nur bei einem aktivem LVM-System vorhanden ist. Ist das Modul nicht vorhanden, müssen Sie den Kernel mit der LVM-Funktion neu kompilieren. Sehen Sie auch dazu das Kapitel über die  Neukompilierung eines Kernels von SelfLinux. Zusätzlich zur Kernel-Funktion ist noch das Programmpaket lvm für die LVM-Kommandos notwendig, das sich, falls es nicht installiert sein sollte, meistens auf einer der CDs Ihrer Linux-Distribution befindet.

Einführung
^^^^^^^^^^

LVM-System starten
""""""""""""""""""

Um die LVM-Funktion des Kernels nutzen zu können, ist es notwendig, das Modul lvm-mod zu laden. Dies geschieht mit folgendem Befehl.
root@linux ~# modprobe lvm-mod

Die LVM-Kommandos setzen die Datei /etc/lvmtab und das Verzeichnis /etc/lvmtab.d voraus, die man vorher mit dem Befehl vgscan erstellen kann. Die beiden Dateien beinhalten Informationen über die vorhandene LVM-Konfiguration. Mit dem Befehl vgchange werden eventuelle Volume Groups aktiviert. Bei vielen Distributionen werden die beiden folgenden Befehle während des Systemstarts ausgeführt und sind damit nicht unbedingt notwendig. Sehen Sie dazu auch den Abschnitt  "LVM beim Booten und Shutdown".
root@linux ~# vgscan -v
root@linux ~# vgchange -a y

LVM-System einrichten
"""""""""""""""""""""

Physical Volume einrichten

Das LVM-System basiert auf drei Stufen: dem Physical Volume, der Volume Group und dem Logical Volume. Genauere Beschreibungen dieser und anderer Begriffe finden sie im Anhang unter  "Fachbegriffe" erläutert.

Als erstes müssen Sie eine bestehende Partition mit der Partitions-ID 8e für LVM kennzeichnen. Dazu führen Sie als Benutzer root cfdisk gefolgt mit der Angabe der betreffenden Festplatte aus. cfdisk ist eine komfortablere Variante von fdisk.
root@linux ~# cfdisk /dev/hdb

Danach wählt man mit den Cursortasten vertikal die gewünschte Partition (zum Beispiel /dev/hdb5) und danach horizontal die Option "Type" um die Partitions-ID 8e festzulegen. Mit der Option "Write" werden die Änderungen in der Partitionstabelle eingetragen.

Danach kann man auf dieser Partition ein Physical Volume einrichten. Die LVM-Kommandos setzen die Dateien /etc/lvmtab und /etc/lvmtab.d voraus, die man gegebenenfalls mit dem Befehl
root@linux ~# vgscan -v

erstellen kann. Mit dem Befehl
root@linux ~# pvcreate /dev/hdb5

kann dann das Physical Volume erstellt werden. Theoretisch wäre eine Volume Group auch mit nur einem Physical Volume möglich, hier erstellen wir jedoch noch eine zweite, die wir später in der Volume Group zusammenfügen.
root@linux ~# pvcreate /dev/hdb6

Voraussetzung ist natürlich wieder, dass diese Partition die ID 8e hat.

Volume Group einrichten

Die Volume Group stellt eine Art Speicherpool dar, aus der man eine oder mehrere Logical Volumes, also virtuelle Partitionen, erstellen kann. Zusätzlich zum Kommando vgcreate und den Physical Volumes muss der gewünschte Name, hier volg1, der Volume Group angegeben werden.
root@linux ~# vgcreate volg1 /dev/hdb5 /dev/hdb6

Danach befindet sich im Verzeichnis /dev das neue Unterverzeichnis volg1 für die betreffende Volume Group.

Logical Volume einrichten

Nun kann man mit der gesamten Volume Group volg1, oder auch nur mit einem Teil davon ein Logical Volume erstellen. Zum Kommando lvcreate muss man die gewünschte Größe, den Namen des Logical Volume und die Volume Group angeben. Hier wird der Name logv1 und die Größe 1000 Megabyte verwendet.
root@linux ~# lvcreate -n logv1 -L 1000M volg1

Damit wird die neue Device-Datei /dev/volg1/logv1 erstellt, über diese man auf die virtuelle Partition zugreifen kann. Genau nach dem gleichen Verfahren wie etwa auf die gewöhnliche Partition /dev/hda1.

Um auf dieser Partition auch Daten abspeichern zu können, ist auch hier ein Dateisystem wie ext2 oder reiserfs erforderlich.
root@linux ~# mkfs -t ext2 /dev/volg1/logv1

Das Dateisystem wird dann über ein Verzeichnis in den Verzeichnisbaum eingehängt.
root@linux ~# mkdir /lvm-test
root@linux ~# mount -t ext2 /dev/volg1/logv1 /lvm-test

Nun können Sie im neu erstellten Verzeichnis /lvm-test Daten abspeichern. Bei Bedarf können sie mit umount die Partition auch wieder aus dem Verzeichnisbaum aushängen.
root@linux ~# umount /lvm-test

LVM-System vergrößern und verkleinern
"""""""""""""""""""""""""""""""""""""

Logical Volume vergrößern und verkleinern

Wie schon gesagt lässt sich mit LVM eine Partition nachträglich vergrößern und auch verkleinern. Möchte man das zuvor angelegte Logical Volume mit der Größe von 1000 Megabyte vergrößern, kann man dies mit lvextend erledigen. Dazu gibt man einfach die neue Größe mit der Option -L direkt an. Alternativ könnte man auch mit -L+300M die neue Größe relativ zur bestehenden Größe angeben. Aufgrund der Größe von 4 Megabyte der Physical Extents, können die tatsächlisch erzeugten Größen der Logical Volumes etwas abweichen, da die erzeugten Logical Volumes damit immer nur ein Vielfaches von 4 MB groß sein können. Um diese Abweichung zu umgehen, können Sie beim Anlegen einer Volume Group die Größe der Physical Extents explizit kleiner angeben. Sehen Sie dazu auch den Abschnitt  "Volume Group mit spezieller PE-Größe".
root@linux ~# lvextend -L 1300M /dev/volg1/logv1

Jetzt wurde erst die virtuelle Partition, also das Logical Volume vergrößert. Zusätzlich muss man nun auch das darin enthaltene Dateisystem vergrößern. Zuvor muss es allerdings mit umount aus dem Verzeichnisbaum entfernt und noch mit e2fsck auf Fehler überprüft werden.
root@linux ~# umount /lvm-test
root@linux ~# e2fsck -f /dev/volg1/logv1
root@linux ~# resize2fs /dev/volg1/logv1
root@linux ~# mount -t ext2 /dev/volg1/logv1 /lvm-test

Umgekehrt können Sie mit resize2fs das Dateisystem auch verkleinern, indem Sie die neue Größe in Blöcken (per Default 1024 Byte) angeben. Im Beispiel wird das Logical Volume auf 500 Megabyte verkleinert. Beachten Sie unbedingt, dass Sie erst das Dateisystem und danach das Logical Volume mit lvreduce verkleinern. Würden Sie erst das Logical Volume mit lvreduce verkleinern, gehen die darin enthaltenen Daten verloren!
root@linux ~# umount /lvm-test
root@linux ~# e2fsck -f /dev/volg1/logv1
root@linux ~# resize2fs /dev/volg1/logv1 512000
root@linux ~# lvreduce -L-800M /dev/volg1/logv1
root@linux ~# mount -t ext2 /dev/volg1/logv1 /lvm-test

Um komfortabler zu arbeiten, gibt es das Kommando e2fsadm, das alle vorher beschriebenen Schritte wie lvextend, lvreduce, e2fsck und resize2fs zusammen ausführt. Wie der Name schon andeutet, funktioniert das Programm nur bei dem Dateisystem ext2. Falls das Programm nicht in Ihrer Distribution enthalten ist, können Sie es unter en http://e2fsprogs.sourceforge.net/ downloaden. Das folgende Kommando vergrößert zum Beispiel das Logical Volume auf 800 Megabyte. Zuvor muss es allerdings wieder mit umount ausgehängt werden.
root@linux ~# umount /lvm-test
root@linux ~# e2fsadm -L 800M /dev/volg1/logv1
root@linux ~# mount -t ext2 /dev/volg1/logv1 /lvm-test

Bei der Verkleinerung verfährt man in gleicher Weise.
root@linux ~# umount /lvm-test
root@linux ~# e2fsadm -L 500M /dev/volg1/logv1
root@linux ~# mount -t ext2 /dev/volg1/logv1 /lvm-test

Volume Group vergrößern und verkleinern

Da auch der Speicherplatz der Volume Group irgendwann belegt ist und man damit kein Logical Volume mehr anlegen oder vergrößern kann, ist es möglich, auch eine Volume Group mit dem Befehl vgextend zu vergrößern. Man muss nur eine beliebige freie Partition wie in Abschnitt  "Physical Volume einrichten" als Physical Volume einrichten und es der Volume Group zufügen.
root@linux ~# pvcreate /dev/hdb7
root@linux ~# vgextend volg1 /dev/hdb7

Mit vgdisplay kann man sich dann die neue Größe ansehen.
root@linux ~# vgdisplay /dev/volg1

Möchte man eine Volume Group verkleinern, kann man mit dem Befehl
root@linux ~# vgreduce -a volg1

alle freien Physical Volumes aus der Volume Group entfernen. Um ein bestimmtes Physical Volume zu entfernen, muss man den genauen Pfad dessen angeben. Vorher kann man, falls erwünscht, mit dem Befehl pvdisplay -v überprüfen, ob das betreffende Physical Volume Daten enthält oder nicht.
root@linux ~# pvdisplay -v /dev/hdb7
root@linux ~# vgreduce volg1 /dev/hdb7

Voraussetzung ist immer, dass auf dem betreffenden Physical Volume keine Daten enthalten sind. Mit dem Kommando pvmove kann man vorher gegebenenfalls die Daten auf ein anderes Physical Volume verschieben. Sehen Sie dazu auch den Abschnitt  "Daten von einem PV zum anderen PV verschieben".

LVM-System beenden
""""""""""""""""""

Um das LVM-System ordungsgemäß zu beenden, müssen Sie alle Logical Volumes mit umount aus dem Verzeichnisbaum aushängen und danach vgchange ausführen.
root@linux ~# umount /lvm-test
root@linux ~# vgchange -a n

Am komfortabelsten ist es, die Befehle für das Starten und Beenden des LVM-Systems innerhalb des Init-V-Prozesses einzubinden, um nicht immer manuell nach dem Systemstart das LVM-System zu aktivieren. Sehen Sie dazu auch den Abschnitt  "LVM beim Booten und Shutdown". 

Weiterführung
^^^^^^^^^^^^^

LVM beim Booten und Shutdown
""""""""""""""""""""""""""""

Um LVM gleich nach dem Systemstart zur Verfügung zu haben, muss dies innerhalb des Init-V-Prozesses gestartet werden. Bei SuSE ist der Befehl vgchange -a y bereits in /etc/init.d/boot enthalten. Um das LVM-System ordnungsgemäß zu beenden, ist noch vgchange -a n in der Datei /etc/init.d/halt enthalten, die beim Herunterfahren ausgeführt wird. Auch bei Mandrake sind in den aktuellen Versionen die entsprechenden Befehle integriert. Bei Red Hat muss unter Umständen noch nachgebessert werden. Sehen Sie dazu auch das en LVM-HOWTO auf der en LVM-Website, aufgelistet im Literaturverzeichnis. Um zu überprüfen, ob Ihre Distribution ebenfalls schon beim Start LVM aktiviert, führen Sie als root lsmod aus. Ist in der aufgeführten Liste das Modul lvm-mod enthalten, ist dies der Fall. Alternativ können Sie auch das Verzeichnis /proc/lvm, das nur bei aktiviertem LVM existiert, aufrufen.

Danach können Logical Volumes genauso wie herkömmliche Partitionen mit Hilfe eines beliebigen Editors in /etc/fstab eingefügt werden, damit diese automatisch beim Booten in den Verzeichnisbaum eingehängt werden. Gehen Sie dabei sehr vorsichtig vor, bei eventuellen Fehleintragungen könnte es sonst sein, dass Ihr System nicht mehr startet.
/etc/fstab

/dev/volg1/logv1  /lvm-test   ext2   defaults   0  2
     

Danach stehen die Partitionen wie bei dem oben genannten Beispiel unter dem Verzeichnis /lvm-test zur Verfügung.

Daten von einem PV zum anderen PV verschieben
"""""""""""""""""""""""""""""""""""""""""""""

Um Daten von einem Physical Volume zu einem anderen Physical Volume zu verschieben, um zum Beispiel die betreffende Partition danach aus der Volume Group zu entfernen, gibt es das Kommando pvmove. Mit dem folgenden Befehl werden alle Daten vom Physical Volume /dev/hdb6 auf den freien, noch zur Verfügung stehenden Platz der anderen Physical Volumes der gleichen Volume Group verschoben. Voraussetzung ist allerdings, dass die restlichen Physical Volumes der Volume Group noch genügend Speicherplatz zur Verfügung stellen, um diese Daten aufnehmen zu können.
root@linux ~# pvmove -v /dev/hdb6

Danach könnte man mit
root@linux ~# vgreduce volg1 /dev/hdb6

die Partition aus der Volume Group entfernen und anderweitig benutzen. Um die Daten auf ein bestimmtes Physical Volume zu verschieben, gibt man dieses als zweites Physical Volume an.
root@linux ~# pvmove -v /dev/hdb6 /dev/hdb7

VG und LV umbenennen
""""""""""""""""""""

Um eine Volume Group oder ein Logical Volume umzubenennen, gibt es die beiden Kommandos vgrename und lvrename.
root@linux ~# vgrename /dev/volg1 /dev/volgroup1
root@linux ~# lvrename /dev/volgroup1/logv1 /dev/volgroup1/logvol1

Danach müssen Sie eventuell den Eintrag in der Datei /etc/fstab ändern.

Volume Group mit spezieller PE-Größe
""""""""""""""""""""""""""""""""""""

Beim Anlegen einer Volume Group besteht die Möglichkeit die Größe der Physical Extents vorzugeben. Standardmäßig ist die Größe von 4 Megabyte eingestellt. Um eine selbst definierte Größe zu erhalten, kann man dies beim Erstellen einer Volume Group mit der Option -s angeben. Es sind Größen von 8 Kilobyte bis 16 Gigabyte möglich.
root@linux ~# vgcreate -s 8k volg2 /dev/hdb7

Nachträglich lässt sich die Größe der Physical Extents nicht ändern. Da je Logical Volume nur 65563 Physical Extents verwaltet werden können, beschränkt die Größe der Physical Extents auch die Größe der Logical Volumes.

Informationen abrufen über PV, VG, LV
"""""""""""""""""""""""""""""""""""""

Um nähere Details zu Physical Volumes, Volume Groups oder Logical Volumes zu erhalten, gibt es die Kommandos pvdisplay, vgdisplay und lvdisplay.
root@linux ~# pvdisplay /dev/hdb5
root@linux ~# vgdisplay /dev/volg1
root@linux ~# lvdisplay /dev/volg1/logv1

Ergänzend gibt es noch Scan-Kommandos, um das System nach LVM-Volumes etc. abzusuchen und aufzulisten.
root@linux ~# pvscan

Dieser Befehl erstellt eine Liste über alle Physical Volumes.
root@linux ~# vgscan

Dieser Befehl erstellt eine Liste aller Volume Groups. Daneben werden die notwendigen Dateien /etc/lvmtab und /etc/lvmtab.d erzeugt.
root@linux ~# lvscan

Dieser Befehl erstellt eine Liste aller Logical Volumes.

LV oder VG löschen
""""""""""""""""""

Mit den beiden Kommandos lvremove und vgremove lassen sich Logical Volumes beziehungsweise Volume Groups aus dem System entfernen. Zu beachten ist, dass nur ausgehängte Logical Volumes entfernt werden können. Führen Sie dazu folgende Befehle aus. Dabei muss die entsprechende Volume Group noch aktiv sein. Dieses Beispiel geht davon aus, dass das Logical Volume über das Verzeichnis /lvm-test gemountet ist.
root@linux ~# umount /lvm-test
root@linux ~# lvremove /dev/volg1/logv1

Nach dem Deaktivieren der Volume Group, können Sie dann schließlich auch mit dem Befehl vgremove die Volume Group löschen, vorausgesetzt es existieren keine weiteren Logical Volumes innerhalb dieser Volume Group mehr.
root@linux ~# vgchange -a n /dev/volg1
root@linux ~# vgremove /dev/volg1

LVM für die Root-Partition
""""""""""""""""""""""""""

Um auch LVM für die Root-Partition nutzen zu können, ist es notwendig, dass der Kernel das Modul lvm-mod schon vor dem Zugriff auf die Root- Partition geladen hat, sonst ist ein Zugriff darauf nicht möglich. Dafür ist ein Kernel mit fest integriertem LVM-Modul oder eine Initial-RAM-Disk erforderlich. Die aktuellen Versionen der SuSE-Distribution bieten bei der Installation die Möglichkeit, auch eine LVM-Partition für die Root-Partition zu erstellen.

LVM auch für die Root-Partition zu verwenden, birgt einige Gefahren in sich und kann Ihr ganzes System unbrauchbar machen. Außerdem kann es bei späteren Distributions-Updates zu Komplikationen kommen. Zusätzlich kann es bei einer Beschädigung des Dateisystems der Root-Partition aufwändiger sein, dieses wiederherzustellen. Eine Umstellung der Root-Partition auf LVM ist deshalb nur erfahrenen Linux-Anwendern zu empfehlen. Daher rate ich in der Regel davon ab. Des Weiteren übernehme ich keine Garantie für die hier beschriebene Vorgehensweise. Wollen Sie dennoch Ihre Root-Partition auf LVM aufsetzen und sind sich der Gefahren bewusst, sollten Sie vorher unbedingt zur Sicherheit ein Backup Ihrer Daten und der Systempartition anlegen.

Eine der einfachsten und sichersten Möglichkeiten LVM für die Root-Partition einzurichten, ist die Verwendung einer so genannten Live-Distribution. Dies ist eine Linux-Distribution, die komplett von einer CD läuft und die keiner Installation bedarf und damit vollkommen ohne Festplatte auskommt. Dies ermöglicht ein komfortables Arbeiten an der inaktiven Root-Partition. Bei anderen Verfahren wären Komplikationen mit der gemounteten Root-Partition nicht auszuschließen, da die meisten Programme, wie zum Beispiel resize2fs, ein ausgehängtes Dateisystem voraussetzen, und ein umount kann man nicht einfach auch für die Systempartition anwenden.

Eine weit verbreitete und sehr empfehlenswerte Distribution dieser Art ist Knoppix, das Sie unter de http://www.knopper.net/knoppix/ beziehen können. Das hier beschriebene Verfahren bezieht sich ausschließlich auf die Verwendung einer Live-Distribution. Um Knoppix zu starten, booten Sie von der Knoppix-CD. Mit der Eingabetaste am Bootprompt gelangen Sie zum KDE-Desktop, mit der Eingabe von knoppix 2 in eine Textkonsole. Da Knoppix ein vollständiges Linux-Betriebssystem ist, bringt es eine vielfältige Auswahl an Tools mit, unter anderem das Programm en Partition Image, das Sie mit partimage in einer Shell starten. Damit können Sie gleich die dringend zu empfehlende Sicherung Ihrer Root-Partition durchführen, in dem Sie ein Abbild dieser Partition in eine Image-Datei speichern. Diese Image-Datei speichern Sie dann am besten auf einen gesonderten Datenträger. Falls etwas schief gehen sollte, können Sie den Zustand der Root-Partition zum Zeitpunkt der Sicherung wiederherstellen, indem Sie die Image-Datei wieder zurückspielen. Sichern Sie zusätzlich noch Ihre Datenpartitionen.

Das hier beschriebene Verfahren geht von folgender Systemkonfiguration auf einer 1,6 GB großen Festplatte aus:
/dev/hda1     swap   (Swap-Partition)    192 MB
/dev/hda2       /    (Root-Partition)    1,5 GB
/dev/hda3     boot   (Boot-Partition)    20  MB

Haben Sie noch eine freie Partition inklusive entsprechendem Speicherplatz, entfällt die folgende Verkleinerung der Root-Partition. Sie können auf der entsprechenden Partition dann gleich ein Logical Volume für die Root-Partition anlegen.

Damit die Root-Partition auf LVM aufsetzen kann, ist es erforderlich, erst ein neues Logical Volume zu erstellen und dann die komplette Root-Partition darauf zu kopieren. In diesem Falle wird dazu die Root-Partition verkleinert, um neuen Speicherplatz frei zu machen. Aus diesem Speicherplatz wird dann eine neue Partition erstellt, die später die Root-Partition aufnehmen wird. Daher darf hier die aktuelle Root-Partition nicht einmal die Hälfte der Partition in Anspruch nehmen. Zum Verkleinern der Partition nehmen Sie am besten das Programm en GNU Parted. Gestartet wird es auf der Kommandozeile mit parted gefolgt von dem jeweiligen Device.
root@linux ~# parted /dev/hda
   (parted) p      # zeigt die aktuelle Partitionstabelle an
Minor   Start     End       Type       Filesystem       Flags

1       0,031    192,137    primary     linux-swap
3      192,938   214,539    primary         ext2          boot
2      252,000  1549,406    primary         ext2

Um die Root-Partition zu verkleinern, gibt man in Parted die Partitionsnummer, hier zum Beispiel die Nummer zwei, und den Start sowie das Ende der Partition in Megabyte an. Achten Sie darauf, dass der freiwerdende Speicherplatz etwas größer wird als die Root-Partition, damit alle Daten später von der Systempartition dorthin kopiert werden können.
   (parted) resize 2 252.000 850

Danach muss aus dem freigewordenen Speicherplatz eine neue Partition angelegt werden. Das folgende Kommando erstellt eine neue primäre Partition aus dem restlichen Plattenplatz nach der Root-Partition.
   (parted) mkpart primary 851 1550
   (parted) p
Minor   Start     End       Type       Filesystem       Flags

1       0,031    192,137    primary     linux-swap
3      192,938   214,539    primary         ext2         boot
2      252,000   850,000    primary         ext2
4      850,500   1549,406   primary                      lvm    (parted) q

Nach dem Verlassen von en GNU Parted starten Sie Ihr altes System neu.
root@linux ~# reboot

Nach dem Neustart erstellen Sie ein Logical Volume ohne Dateisystem aus der gesamten Größe der neuen Partition /dev/hda4. Sehen Sie dazu gegebenenfalls den Abschnitt  "LVM-System einrichten". Nun müssen Sie die komplette Root-Partition auf das neue Logical Volume kopieren. Dazu booten Sie erneut Knoppix. In diesem Beispiel wurde das Logical Volume rootlv genannt, dieses befindet sich in der Volume Group rootvg.
root@linux ¯# root@linux # vgscan
root@linux ~# vgchange -a y
root@linux ~# dd if=/dev/hda2 of=/dev/rootvg/rootlv
root@linux ~# sync

Nach dem Kopiervorgang, der einige Zeit in Anspruch nehmen kann, mounten Sie das Logical Volume unter dem Verzeichnis /root-lvm und ändern eine Zeile in der Datei /root-lvm/etc/fstab mit dem Editor  Emacs.
root@linux ~# mkdir /root-lvm
root@linux ~# mount /dev/rootvg/rootlv /root-lvm
root@linux ~# emacs /root-lvm/etc/fstab

Diese Zeile
/root-lvm/etc/fstab

/dev/hda2              /        ext2       defaults     1  1
     

wird geändert zu dieser
/root-lvm/etc/fstab

/dev/rootvg/rootlv     /        ext2       defaults     1  1
     

Danach booten Sie wieder Ihr altes System. Falls der Kernel Ihrer Distribution ohne fest integrieter LVM-Funktion besteht, müssen Sie noch eine Initial-RAM-Disk erstellen, aus der beim Systemstart das LVM-Modul geladen wird. Beachten Sie, dass das folgende Kommando nur eine Initial-RAM-Disk mit einem LVM-Modul erstellt. Bei manchen Distributionen ist es erforderlich, vorher noch das Programmpaket binutils zu installieren.
root@linux ~# lvmcreate_initrd

Danach sollte sich die Initial-RAM-Disk im Verzeichnis /boot befinden. Als nächstes müssen Sie Ihren Boot-Manager wie zum Beispiel Lilo anpassen. Fügen Sie dazu etwa folgenden Eintrag in die Datei /etc/lilo.conf hinzu.
/etc/lilo.conf

image   = /boot/vmlinuz
label   = lvm
root    = /dev/rootvg/rootlv
initrd  = /boot/initrd.gz
ramdisk = 8192
     

Kopieren Sie anschließend am besten gleich diese Datei auch in die neue Root-Partition. Danach können Sie lilo ausführen.
root@linux ~# mkdir /root-lvm
root@linux ~# mount /dev/rootvg/rootlv /root-lvm
root@linux ~# cp /etc/lilo.conf /root-lvm/etc/lilo.conf
root@linux ~# lilo

Danach starten Sie Ihr System neu und booten unter der Angabe von lvm am Lilo-Bootprompt von der neuen Root-Partition. Wenn das Booten von dem neuen Logical Volume, auf der sich nun die Root-Partition befindet, gelingt und alles einwandfrei funktioniert, können Sie die alte Root-Partition als Physical Volume definieren und der Volume Group rootvg hinzufügen. Vorher sollten Sie jedoch Ihr neues System gründlich prüfen. Wollen Sie die Root-Partition mittels LVM nachträglich vergrößern, können Sie mit dem Befehl lvextend unter Ihrem neuen System die Partition vergrößern. Um danach auch das darin befindliche Dateisystem fehlerfrei vergrößern zu können, müssen Sie wieder Knoppix booten und resize2fs dann dort für die inaktive Root-Partition ausführen.

LVM kombiniert mit RAID
"""""""""""""""""""""""

LVM im RAID-Level 0

LVM unterstützt den RAID-Level 0, auch Stripe-Set genannt, bei dem die Daten alternierend auf verschiedene Festplatten in geteilte Datenblöcke gespeichert werden. Dies führt zu einem ernormen Geschwindigkeitszuwachs, vor allem beim Lesezugriff, bei dem sich die Datenrate fast verdoppeln kann. Sehen Sie dazu auch das  RAID-HOWTO von SelfLinux. Um RAID 0 unter LVM nutzen zu können, muss auf zwei oder mehr Festplatten jeweils ein Physical Volume eingerichtet werden. Danach fasst man diese als eine Volume Group zusammen und erstellt mit folgendem Kommando daraus ein Logical Volume.
root@linux ~# lvcreate -n lvstriped -L 1000M -i 2 volg1

Die Option -i 2 bewirkt, dass das Logical Volume aus zwei Physical Volumes erstellt wird. Um keine Geschwindigkeitseinbußen zu bekommen, müssen Sie darauf achten, dass alle Physical Volumes immer auf verschiedene Festplatten liegen.

LVM und höhere RAID-Level

LVM kann man auch mit anderen RAID-Leveln kombinieren. Hierzu richtet man auf dem betreffenden /dev/md*-Device ein Physical Volume ein und benutzt dieses wie gewohnt.

LVM basierend auf Loopback-Devices
""""""""""""""""""""""""""""""""""

Wie schon erwähnt lasst sich LVM alternativ zu Partitionen auch mit Loopback-Devices verwenden. Dies hat den Vorteil die Festplatte nicht umpartitionieren zu müssen, und eignet sich somit ideal um LVM erst einmal zu testen. Jedoch sollte man wegen der Datensicherheit und Performance bei späteren ernsthaften Verwendungen von LVM, wenn möglich, richtige Partitionen verwenden. Denn mit Loopback-Devices werden die Daten innerhalb einer Datei abgelegt, die wiederum in einem Dateisystem einer gewöhnlichen Partition liegt. Ein löschen dieser Datei würde dann zum Verlust aller Daten führen. Zu beachten ist, dass eine Volume Group eine Mindestgröße von 20 Megabyte haben muss.

Als erstes ist es notwendig die erforderliche Datei als Container für die Daten zu erstellen. Dies geschieht mit dem folgenden Befehl.
root@linux ~# dd if=/dev/zero of=/lvm-test/.lvmcontainer bs=1024 count=51200

Dieser Befehl erstellt eine 50 Megabyte große Datei im Verzeichnis /lvm-test. Optional habe ich diese Datei mit einem Punkt am Anfang versehen, damit sie als versteckte Datei in der Normalansicht nicht zu sehen ist. Als nächstes muss nun diese Datei mit einem Loopback-Device verbunden werden.
root@linux ~# losetup /dev/loop1 /lvm-test/.lvmcontainer

Danach kann auf diese Datei über /dev/loop1 als gewöhnliches Block-Device zugegriffen werden, und darauf wie auf einer herkömmlichen Partition ein Physical Volume erstellt werden. Falls das Loopback-Device nicht mehr benötigt wird kann es mit
root@linux ~# losetup -d /dev/loop1

wieder von der Datei gelöst werden.

Dieser Eintrag in der Datei /etc/fstab würde schon beim Systemstart automatisch die Datei /lvm-test/.lvmcontainer per Loopback-Device über das Verzeichnis /lvm-test mounten.
/etc/fstab

/lvm-test/.container    /lvm-test   ext2    defaults,loop    0    0
     
Logical Volume für Swap-Partition
"""""""""""""""""""""""""""""""""

Man kann ein Logical Volume auch als Swap-Partition benutzen. Dazu muss man lediglich das betreffende Logical Volume mit mkswap formatieren und mit swapon aktivieren. Falls jedoch die Volume Group, innerhalb der das Logical Volume erstellt wurde, aus mehreren Partitionen besteht, ist es möglich, dass damit auch die LVM-Swap-Partition über mehrere Partitionen verteilt ist, was den Zugriff auf die Swap-Partition verlangsamt.
root@linux ~# lvcreate -n swaplv -L 500M volg1
root@linux ~# mkswap /dev/volg1/swaplv
root@linux ~# swapon /dev/volg1/swaplv

Damit die Swap-Partition automatisch beim Systemstart aktiviert wird, tragen Sie folgende Zeile in die Datei /etc/fstab ein.
/etc/fstab

/dev/volg1/swaplv      swap      swap     defaults     0   0
     

Existiert bereits eine Swap-Partition, und wollen Sie diese Swap-Partition noch zusätzlich weiter verwenden, können Sie mit der unten angegebenen Option pri=1 bewirken, dass beide Swap-Partitionen gleichwertig behandelt werden. Dies kann zu einer Performancesteigerung nach dem Prinzip von RAID 0 führen, falls beide Partitionen auf verschiedenen Festplatten liegen. Zwei Swap-Partitionen auf einer Festplatte sollte man vermeiden, da sich dann beide gegenseitig ausbremsen würden.
/etc/fstab

/dev/volg1/swaplv      swap       swap     defaults,pri=1     0   0
/dev/hda8              swap       swap     defaults,pri=1     0   0
   
Snapshot eines Logical Volume
"""""""""""""""""""""""""""""

Ein Snapshot ist eine Kopie, die man von einem Logical Volume als Backup anlegen kann. Dazu dient wiederum der Befehl lvcreate mit der speziellen Option -s oder --snapshot.
root@linux ~# lvcreate -L 500M --snapshot -n mysnap /dev/volg1/logv1

Danach steht der identische Inhalt des Logical Volume /dev/volg1/logv1 unter /dev/volg1/mysnap bereit. Dabei ist zu beachten, dass der Snapshot einen Teil des Speicherplatzes der Volume Group belegt. Die Option -L 500M gibt nicht etwa die eigentliche Größe des Snapshot an, sondern wie viel sich am Original ändern darf, bevor der Snapshot ungültig wird.

VG auf anderen Rechner transferieren
""""""""""""""""""""""""""""""""""""

Es besteht die Möglichkeit, vorhandene lokale Volume Groups auf einen anderen Computer weiterzubenutzen, falls man die lokale Festplatte, auf der sich die betreffenden Physical Volumes einer Volume Group befinden, in den anderen Rechner einbauen will. Vorher muss man allerdings die entsprechende Volume Group aus dem System entfernen. Um dies zu bewirken, gibt es den Befehl vgexport, der, nachdem man die entsprechende Volume Group deaktiviert hat, diese ordnungsgemäß aus dem System entfernt. Man kann zudem gegebenenfalls noch mit pvscan überprüfen, welche Physical Volumes zu welcher Volume Group gehören.
root@linux ~# pvscan
root@linux ~# umount /lvm-test
root@linux ~# vgchange -a n /dev/volg1
root@linux ~# vgexport /dev/volg1

Ist die Festplatte in den anderen Rechner eingebaut, kann man analog dazu mit vgimport, der Angabe eines Namens und der Physical Volumes die Volume Group auf diesem Computer wieder weiterverwenden. Voraussetzung ist natürlich, dass auch dort ein funktionierendes LVM-System vorhanden ist.
root@linux ~# vgimport newvg /dev/hdb5 /dev/hdb6
root@linux ~# vgchange -a y /dev/newvg
root@linux ~# mkdir /lvm-test
root@linux ~# mount -t ext2 /dev/newvg/logv1 /lvm-test

Dateisystem im Betrieb vergrößern
"""""""""""""""""""""""""""""""""

Die Möglichkeit ein Dateisystem im laufendem Zustand zu vergrößern ist vor allem im Server-Betrieb sehr praktisch, da man die Downtime dieses Servers sehr gering halten kann, und dieser sehr schnell wieder zur Verfügung steht. Das Programm resize2fs ist nur in der Lage die Größe eines Dateisystems zu verändern, wenn dieses gerade nicht im Verzeichnisbaum gemountet ist. Daneben gibt es noch zusätzlich das Programm ext2online, das ext2-Dateisysteme auch im gemountetem Zustand verändern kann. Dafür ist jedoch zur Zeit noch ein Kernel-Patch erforderlich, den man inklusive dem Programm unter en http://sourceforge.net/projects/ext2resize/ downloaden kann. Veränderungen am Kernel sind jedoch mit Vorsicht auszuführen. Aktuell gibt es diese Möglichkeit auch für ext3. Alternativ kann man das Dateisystem reiserfs verwenden, das man auch im gemountetem Zustand vergrößern kann. Eine Verkleinerung dieses Dateisystems ist jedoch auch hier nur möglich, wenn es vorher mit umount ausgehängt wurde. Diese Funktion ist allerdings noch relativ neu, eventuelle Bugs sind deswegen nicht auszuschließen.
root@linux ~# lvcreate -n logv2 -L 500M volg1
root@linux ~# mkfs -t reiserfs /dev/volg1/logv2
root@linux ~# mount -t reiserfs /dev/volg1/logv2 /lvm-test
root@linux ~# lvextend -L 1000M /dev/volg1/logv2
root@linux ~# resize_reiserfs /dev/volg1/volg2

Um das Dateisystem wieder zu verkleinern, muss es vorher mit umount ausgehängt werden.
root@linux ~# lvreduce -L 500M /dev/volg1/logv2
root@linux ~# umount /lvm-test
root@linux ~# resize_reiserfs /dev/volg1/logv2
root@linux ~# mount -t reiserfs /dev/volg1/logv2 /lvm-test

Grafische Benutzeroberflächen für LVM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Bei den aktuellen großen Distributionen ist bereits bei den mitgelieferten Konfigurationstools ein grafischer Partitionseditor enthalten, mit dem sich ein LVM-System komfortabler einrichten lässt. Bei SuSE ist dies beispielsweise das LVM-Modul von YaST. Des Weiteren bietet Webmin unter en http://www.webmin.com noch ein LVM-Feature, das ich jedoch nicht getestet habe. Ein weiteres, sehr gutes GUI für LVM ist das so genannte Enterprise Volume Management System EVMS unter en http://evms.sourceforge.net/. Es bietet eine einfache und übersichtliche Benutzeroberfläche, um Volume Groups, Logical Volumes etc. anzulegen. Das Programm unterstützt nicht nur das LVM-System unter Linux, sondern auch den LVM vom Unix-Derivat AIX von IBM. Allen hier beschriebenen Tools ist gemeinsam, dass man das Grundprinzip von LVM verstanden haben sollte. Erfahrene Anwender bevorzugen aber eher die Kommandozeile, auch wegen eventueller Bugs der Programme.

Anhang
^^^^^^

Kommandoreferenz
""""""""""""""""

Hier sind die wichtigsten, in diesem Kapitel behandelten Kommandos noch einmal zusammengefasst. Es gibt zahlreiche Befehle bezüglich LVM. Für weitere Informationen sehen Sie in den betreffenden Manpages nach.

vgscan
Sucht alle Festplatten nach Volume Groups ab und erzeugt die für LVM-Kommandos benötigten Dateien /etc/lvmtab und /etc/lvmtab.d, in denen wichtige Informationen über das LVM-System auf Ihrem Computer abgespeichert werden.

pvscan
Sucht alle Festplatten nach Physical Volumes ab und listet diese inklusive Größenangabe auf.

pvcreate
Erstellt ein Physical Volume aus einer Partition, die vorher mit der Partitions-ID 8e gekennzeichnet wurde.

vgcreate
Erzeugt aus einem oder mehreren Physical Volumes eine Volume Group.

lvcreate
Erzeugt ein Logical Volume, also eine virtuelle Partition, aus einer Volume Group. Diese ist somit ein Teil einer Volume Group.

lvextend
Vergrößert ein Logical Volume auf die angegebene Größe.

lvreduce
Verkleinert ein Logical Volume auf die angegebene Größe. Vorher muss allerdings das Dateisystem ebenfalls auf die gewünschte Größe verkleinert werden, sonst gehen die darin enthaltenen Daten verloren.

e2fsadm
Dieses Kommando fasst die Befehle lvextend, lvreduce, e2fsck und resize2fs zusammen. Um etwa ein Logical Volume zu vergrößern, müssen Sie nur noch den Befehl, die gewünschte Größe und das Logical Volume angeben.

pvmove
Mit pvmove können Sie die Daten von einem Physical Volume zu einem anderen Physical Volume innerhalb einer Volume Group verschieben, um beispielshalber ein damit leeres Physical Volume aus der Volume Group zu entfernen.

vgreduce
Mit vgreduce können Sie eine Volume Group verkleinern, indem Sie ein leeres Physical Volume angeben, das aus der Volume Group entfernt werden kann.

vgrename, lvrename
Mit diesen Befehlen kann man, wie der Name schon sagt, eine Volume Group oder ein Logical Volume umbenennen.

lvremove, vgremove
Mit diesen Befehlen löschen Sie ein Logical Volume beziehungsweise eine Volume Group.

vgdisplay, pvdisplay, lvdisplay
Zeigen nähere Informationen zu einer Volume Group, einem Physical Volume oder einem Logical Volume an.

vgchange
Mit vgchange aktivieren beziehungsweise deaktivieren Sie alle oder einzelne Volume Groups.

Fachbegriffe
""""""""""""

LVM
Steht für "Logical Volume Manager".

PV
Steht für "Physical Volume" und ist eine gewöhnliche Partition, die von LVM verwaltet wird. Außer Partitionen kann man auch noch Loopback-Devices oder Partitionen, die schon von RAID verwaltet werden, benutzen.

VG
Steht für "Volume Group" und bezeichnet den logischen Zusammenschluss mehrerer "Physical Volumes" zu einem großen Speicherpool. Eine Volume Group kann auch nachträglich noch mit neu angelegten Physical Volumes erweitert werden.

LV
Steht für "Logical Volume" und bezeichnet eine virtuelle Partition, die Teil einer "Volume Group" ist. Ein Logical Volume kann daher aus mehreren gewöhnlichen Partitionen bestehen. Ergänzend zu der Erweiterbarkeit einer Volume Group, kann auch ein Logical Volume nachträglich vergrößert werden. Das Problem mangeldem Speicherplatzes innerhalb einer Partition besteht damit in der Regel unter LVM nicht.

PE
Steht für "Physical Extent" und ist die kleinste verwaltbare Dateneinheit unter LVM. Per Default beträgt die Größe eines "Physical Extent" 4 MB. Jedes Physical Volume besteht aus einer bestimmten Anzahl von diesen Dateneinheiten.

Kernel
Der Kernel bezeichnet den innersten Teil eines Betriebssystems. Dieser hat elementare Aufgaben wie der Speicherverwaltung, Steuerung der Hardware oder der Verwaltung der Prozesse. Vor allem den Linux-Kernel gibt es in sehr vielen unterschielichen Versionen, die sich bei der unterstützten Funktionsvielfalt unterscheiden. Um nachträglich eine Funktion dem Kernel hinzuzufügen, gibt es so genannte Kernel-Patches.

Partition
Der Speicherplatz einer Festplatte lässt sich in mehrere logische Bereiche aufteilen, den so genannten Partitionen.

Partitions-ID
Legt den Typ einer Partition fest (83 für gewöhnliche Linux-Datenpartition, 82 für eine Linux-Swappartition, 8e für LVM-Partition).

Root-Partition
Dies ist das Wurzelverzeichnis / und entspricht der Systempartition bei Linux (bei Windows ist dies c:\).

Swap-Partition
Linux sieht anders als Windows eine separate Partition für die Auslagerungsdatei vor, in der Daten ausgelagert werden, wenn der RAM-Speicher zu klein wird, zudem ermöglicht dies einen Performancegewinn.

Dateisysteme unter Linux
Das am meisten genutzte Dateisystem unter Linux ist das second extended filesystem, kurz ext2. Eine Weiterentwicklung von ext2 ist ext3, das um eine Journaling-Funktion ergänzt wurde, die alle Änderungen am Dateisystem protokolliert, damit sich bei einem Systemcrash schnellstmöglich ein konsistenter Zustand der Daten wiederherstellen lässt. Daneben gibt es noch zahlreiche andere, die sich unter anderem im Umgang mit kleinen und großen Dateien, sowie in der Geschwindigkeit bei Dateioperationen unterscheiden. Ein weiteres häufig verwendetes Dateisystem ist reiserfs, das kleine Dateien platzsparender speichert und zudem eine Journaling-Funktion besitzt.

Loopback-Device
Mittels so genannter Loopback-Devices ist es möglich Dateien wie gewöhnliche Block-Devices anzusprechen. Damit ist es möglich innerhalb einer Datei ein Dateisystem anzulegen und diese wie eine Partition zu nutzen. LVM kann anstatt auf herkömmliche Partitionen auch auf Loopback-Devices aufbauen.

RAID
Steht für "Redundant Array of Independent Disks" und dient zur Erhöhung der Datensicherheit und/oder der Performance, indem mehrere Festplatten zu logischen Einheiten zusammengefasst werden. LVM unterstützt den RAID-Level 0. Außerdem ist es möglich LVM mit RAID zu kombinieren. Sehen Sie dazu auch das  RAID-HOWTO von SelfLinux.

root
Unter Linux ist es manchmal notwendig, als Systembenutzer root bestimmte Befehle auszuführen, da nur dieser uneingeschränkte Nutzungsrechte hat und alle Befehle ausführen darf. Unter Windows NT/2000/XP entspricht dies dem Administrator. Bei systemnahen Aufgaben, wie der Einrichtung von einem Logical Volume Manager, sind in der Regel root-Rechte erforderlich.

mounten
Mit dem Befehl mount hängt man externe Datenträger (Partition, CD-ROM etc.) in den Verzeichnisbaum ein, über die man mittels eines gewählten Verzeichnisses zugreifen kann. Der Befehl umount hängt dieses dann wieder aus. Beispiel:
root@linux ~# mount -t ext2 /dev/hda5 /verzeichnis
root@linux ~# umount /verzeichnis

Verzeichnisstruktur unter Linux
"""""""""""""""""""""""""""""""

Linux kennt wie alle anderen Unix-Derivate keine Laufwerksbuchstaben wie Windows. Festplatten und Partitionen werden direkt durch einfache Verzeichnisse ins Dateisystem eingehängt. Bei Windows 2000 und XP gibt es diese Möglichkeit mit der NTFS-Funktion "Bereitgestellte Laufwerke" auch. Unter DOS gab es dazu den Befehl "join". Der Verzeichnisbaum ist hierarchisch aufgebaut und beginnt mit dem Wurzelverzeichnis /, an dem die Systempartition eingehängt ist (entspricht bei Windows c:\). Für weiterführende Informationen bezüglich der  Verzeichnisstruktur unter Linux, sehen Sie auch dazu das gleichnamige Kapitel von SelfLinux. Wichtige Verzeichnisse sind zum Beispiel:

/mnt (enthält die Unterverzeichnisse über die externe Dateisysteme wie Festplatten oder das CD-ROM eingebunden werden)

/etc (enthält die wichtigsten Konfigurationsdateien)

/dev (enthält die Device-Dateien für den Zugriff auf Hardware-Komponenten)

Laufwerke und Partitionen unter Linux

Auf Hardware-Komponenten, wie zum Beispiel einer Festplatte, wird unter Linux über Device-Dateien, die im Verzeichnis /dev liegen, zugegriffen.

IDE-Laufwerke

    /dev/hda Master am 1. IDE-Kanal
    /dev/hdb Slave am 1. IDE-Kanal
    /dev/hdc Master am 2. IDE-Kanal
    /dev/hdd Slave am 2. IDE-Kanal

SCSI-Laufwerke

    /dev/sda erste SCSI-Festplatte
    /dev/sdb zweite SCSI-Festplatte
    /dev/scd0 erstes SCSI-CD-ROM

Floppy-Laufwerke

    /dev/fd0 erstes Diskettenlaufwerk

Die Zahlen nach den Device-Dateien für Festplatten, wie beispielsweise /dev/hda1, geben die Partition der jeweiligen Festplatte an. Die Zahlen eins bis vier sind für primäre und erweiterte Partitionen reserviert.

    /dev/hda1 primäre Partition (entspricht c:\ bei Windows)
    /dev/hda2 erweiterte Partition
    /dev/hda5 logisches Laufwerk (entspricht d:\ bei Windows)
    /dev/hda6 logisches Laufwerk (entspricht e:\ bei Windows)

Die einzelnen Partitionen werden dann mit dem Befehl mount über ein beliebiges Verzeichnis eingehängt und mit umount gegebenenfalls ausgehängt.

Literaturverzeichnis
""""""""""""""""""""

    Michael Kofler: Linux - Installation, Konfiguration, Anwendung (6. Auflage), Addison-Wesley 2002
    Jochen Hein: Linux Systemadministration - Einrichtung, Verwaltung, Netzwerkbetrieb (4. Auflage), Addison-Wesley 2002
    Richard Heider: LVM Howto - deutsche Version, de http://www.linuxhaven.de/dlhp/HOWTO-test/DE-LVM-HOWTO.html
    AJ Lewis: LVM HOWTO, en http://www.tldp.org/HOWTO/LVM-HOWTO.html


Kontakt und Feedback
""""""""""""""""""""

Wenn Sie irgendwelche Fragen, Anregungen, Kritik oder Ideen bezüglich dieses HOWTO haben, würde ich mich über eine E-Mail freuen.

Markus Hoffmann (  mar.hoff@gmx.net) 

Lizenz

    GPL

Autor

    Markus Hoffmann mar.hoff@gmx.net
	
Formatierung

    Florian Frank florian.frank@pingos.org


