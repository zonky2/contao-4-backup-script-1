# Backup Script für Contao 4 Installationen

## Was macht es

Erstellt ein Backup einer Contao 4 Installation. Erzeugt werden drei Dateien:

* Datenbankdump
* Sicherung der für eine Wiederherstellung benötigten Dateien
* Sicherung des `files/` Verzeichnisses

## Requirements

* bash
* PHP-Cli (passend zur PHP-Version unter der Contao läuft)
* `mysqldump`



## Installation/Verwendung

* Anpassen der Datei `main.sh` an den eigenen Bedarf (siehe Kommentare in der Datei)
* Aufruf der `main.sh` manuell oder periodisch in einem cron-job

```bash
# Projektverzeichnis erstellen (z.B.)
mkdir backup/contao
cd backup/contao
git clone https://github.com/fiedsch/contao-4-backup-script
cp contao-4-backup-script/main.sh ./meinbackup.sh
# meinbackup.sh (oder wie auch immer Du die Datei für Dich passend genannt hast)
# bearbeiten und die zur Contao Installation passenden Parameter setzen.
# Dann meinbackup.sh in einen cron job eintragen.
```



## Spezielle Anpassungen (abhängig vom Provider)

Manche Provider stellen nicht alle im Skript benötigten Befehle bereit (sperren den Zugriff).
Über spezielle Konfigurationsoptionen soll versucht werden, dies zu berücksichtigen.


### All-Inkl

In der `main.sh`
* `TAR` auf den Wert `ptar` setzen
* `OS` auf den Wert `Linux` setzen
* `PURGE_AFTER_DAYS` auf `0` setzen (@mlwebworker: weil der Befehl zum Löschen nicht freigegeben ist. Die Löschung kann dann über Tools → Webspacebereinihung automatisiert werden)


### Mittwald

@zonky2: In der `main.sh`
 * `OS` auf den Wert `Linux` setzen


### Andere Provider

Feedback zu weiteren Providern, bei denen es noch nicht gelöste Probleme gibt gerne
als die Issue in diesem Repository - Danke!


## Restore

* Backup-Dateien in das entsprechende Verzeichnis auf dem Server entpacken
* Datenbankdump einspielen (Datenbank ggf. neu anlegen)
* `composer install`
* Aufruf des Contao Installtools im Browser


## Was noch fehlt

* Liste der gesicherten Dateien auf "ist alles benötigte dabei" prüfen (möglichst viele
  Spezialfälle berücksichtigen; Danke für Feedback/"Issues" falls ihr etwas findet!)
* `mysqldump` ohne (direkte) Angabe des Passworts: `mysqldump --defaults-file=/path-to-file/my.cnf`
und `my.cnf` so:

 ```
 [mysqldump]
 password=my_password
 ```
* Fehlerprüfungen und ggf. -meldungen (siehe [Issue #14](https://github.com/fiedsch/contao-4-backup-script/issues/14))
