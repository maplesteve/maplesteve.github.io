---
layout: post
title: Problem mit Time Machine gelöst!
category: digital
tags: tech howto
---

Nachdem das MacBook durch ein neues MacBook Pro ersetzt wurde, habe ich einen frischen Leoparden installiert, um es für die nächste Benutzerin aufzusetzen. Nach erfolgter Installation, Einrichtung und Umbenennung (*‘Systemeinstellungen – Sharing – Gerätename’*) fehlte als letzter Schritt nur noch die Aktivierung von Time Machine zwecks automatischem Backup auf unser Time Capsule.

Eigentlich total einfach: ‘Systemeinstellungen’ öffnen, ‘Time Machine’ wählen, Time Capsule auswählen fertig.

Denkste.

Das, was wor der Neuinstallation funktionierte, lief nicht mehr. Time Machine beschwert sich nachhaltig mit folgendem Dialog:

> Time Machine:Das Image des Backup-Volumes konnte nicht aktiviert werden.

Hm. O.k. Das Sparsebundle auf der Time Capsule ‘hört’ ja auch noch auf den alten Namen (ich hatte dem macBook ja einen neuen gegeben). Vielleicht bringt das ja hier irgendwas durcheinander. Also Sparsebundle löschen – was nicht das File an sich löscht, sondern es nur deutlich verkleinerte.

Die Erwartungshaltung war, dass Time Machine ein neues Sparsebundle anlegt – was dann auch mal den neuen Namen reflektieren sollte. Passierte aber nicht; stattdessen o.g. Fehlermeldung. Was will TM denn eigentlich aktivieren, wenn noch gar nichts angelegt wurde?
Konnte eigentlich nur daran liegen, dass es noch eine Verknüpfung zwischen altem Sparsebundle bzw. den Time Machine Einstellungen und dem MacBook gibt. Der Name des Sparsebundle besteht aus dem Gerätenamen sowie der MAC-Adresse der Ethernet-Schnittstelle. Das scheint Time Machine nicht automatisch auflösen zu können. Also ‘Terminal’ anwerfen, nach ```/Volumes/Time_Capsule_Name``` wechseln und ein beherztes

```rm -rf \[Name_des_zu_löschenden_Bundles\].sparsebundle```

absetzen. Danach muss dann noch ein weiteres File gelöscht werden, mithelfe dessen Time Machine offenbar eine Verknüpfung zwischen Backup und Rechner herstellt:

```rm -rf .\[MAC-Adresse_des_betreffenden_Rechners\]```

Damit waren alle Spuren der früheren Time Machine Einrichtung erfolgreich beseitigt und TM verrichtete nach dem normalen Setup einwandfrei seinen Dienst.

Fazit:
Apple sollte Time Machine beibringen, auf derartige Situationen vernünftig zu reagieren. Es sollte bemerken, dass eine bestehende Backup-Einrichtung vorhanden ist und anbieten, diese zu löschen.