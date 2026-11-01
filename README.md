# Sichere Dateifreigabe – Pflegedienst Lindenhof

In diesem Projekt habe ich eine zentrale Dateifreigabe für einen fiktiven Pflegedienst mit elf Mitarbeitenden eingerichtet. Ziel war, Dateien übersichtlich abzulegen und den Zugriff auf die jeweiligen Abteilungen zu begrenzen.

Die Umsetzung erfolgte als IHK-Projekt in einer virtuellen Testumgebung.

## Verwendete Technologien

- Windows Server 2025
- VMware Workstation
- Active Directory Domain Services und DNS
- SMB-Freigaben
- NTFS-Berechtigungen und AD-Sicherheitsgruppen
- Zugriffsbasierte Aufzählung (ABE)

## Aufbau

| Server | Aufgabe |
|---|---|
| SRV-DC01 | Domänencontroller und DNS |
| SRV-APP01 | Domänenmitglied und Dateiserver |

**Domäne:** `lindenhof.test`  
**Datenordner:** `D:\Daten`  
**Netzwerkfreigabe:** `\\SRV-APP01\Daten`

## Berechtigungskonzept

Die Zugriffsrechte wurden über die Sicherheitsgruppen `GG_Pflege`, `GG_Verwaltung` und `GG_Leitung` vergeben.

| Ordner | Pflege | Verwaltung | Leitung |
|---|---|---|---|
| Pflegeberichte | Ändern | Kein Zugriff | Ändern |
| Abrechnung | Kein Zugriff | Ändern | Lesen |
| Personal | Kein Zugriff | Kein Zugriff | Ändern |
| Allgemein | Ändern | Ändern | Ändern |

Administratoren und SYSTEM behalten Vollzugriff. Durch ABE werden Ordner ohne Leseberechtigung ausgeblendet.

## Tests und Ergebnisse

Mit verschiedenen Testkonten habe ich geprüft, welche Ordner sichtbar sind und ob Dateien erstellt oder verändert werden können.

Die vorgesehenen Zugriffe funktionierten. Auch der absichtlich eingeschränkte Zugriff wurde bestätigt: Die Leitung konnte den Ordner „Abrechnung“ lesen, dort jedoch keine Dateien erstellen.

## Herausforderungen und Lernerfahrungen

Eine Herausforderung waren geerbte NTFS-Berechtigungen. Ich habe die Vererbung am Hauptordner deaktiviert, die Berechtigungen angepasst und anschließend erneut getestet.

Bei einem Netzwerkproblem wurde ein gestoppter VMware NAT Service als Ursache ermittelt. Nach dem Start des Dienstes wurde die Verbindung wiederhergestellt.

Dabei habe ich besonders das Zusammenspiel von SMB, NTFS und ABE sowie die systematische Fehlersuche vertieft.

## Dokumentation

Die Projektdokumentation mit Berechtigungsmatrix, Testergebnissen und acht Screenshots befindet sich im Ordner `docs`.

## Projektstatus und Ausblick

Die Umsetzung und Tests wurden abgeschlossen. Die virtuellen Maschinen wurden anschließend entfernt; dieses Repository dokumentiert die durchgeführte Arbeit.

Backups, Zugriffsprotokollierung und eine automatische Laufwerksverbindung per GPO sind mögliche Erweiterungen und wurden in diesem Projekt nicht umgesetzt.

**Autorin:** Maryam Pourkhatoun
