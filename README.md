# Handleiding: LineageOS installeren op Samsung Galaxy Tab A7 LTE (SM-T505)

Deze gids beschrijft stap voor stap hoe u LineageOS installeert op een Samsung Galaxy Tab A7 LTE (2020, codenaam `gta4l`, modelnummer SM-T505), gebruikmakend van Linux Mint en Heimdall.

---

## Inhoudsopgave

1. [Voorbereiding](#1-voorbereiding)
2. [Tablet in Download Mode zetten](#2-tablet-in-download-mode-zetten)
3. [Download Mode-statusanalyse](#3-download-mode-statusanalyse)
4. [Verbinding testen met Heimdall](#4-verbinding-testen-met-heimdall)
5. [Recovery en vbmeta flashen](#5-recovery-en-vbmeta-flashen)
6. [Recovery opstarten met juiste toetsvolgorde](#6-recovery-opstarten-met-juiste-toetsvolgorde)
7. [LineageOS installeren via ADB sideload](#7-lineageos-installeren-via-adb-sideload)
8. [Na de installatie](#8-na-de-installatie)

---

## 1. Voorbereiding

- Zorg dat de tablet volledig is opgeladen
- Zet OEM Unlock aan in de ontwikkelaarsopties
- Zet USB-foutopsporing aan
- Controleer dat **'Time-out voor uitzetten ADB-machtiging' ingeschakeld** is
- Controleer dat **'Apps via USB controleren'** is **uitgeschakeld** (voor compatibiliteit)
- Installeer op Linux Mint de correcte versie van Heimdall (v2.0.2 of nieuwer):

```bash
sudo apt install heimdall-flash
```

---

## 2. Tablet in Download Mode zetten

1. Zet de tablet volledig uit
2. Houd **Volume Up + Volume Down** ingedrukt
3. Sluit de USB-kabel aan op de tablet en pc
4. U ziet een blauw scherm met Download Mode-waarschuwing
5. Druk **Volume Up** om verder te gaan naar Download Mode

---

## 3. Download Mode-statusanalyse

Als het toestel correct in Download Mode staat, verschijnt informatie zoals:

| Regel                      | Betekenis                                                                 |
|----------------------------|---------------------------------------------------------------------------|
| RPMB fuse Not Set         | Normaal, opslagbeveiliging nog niet gefuseerd                             |
| RPMB PROVISIONED          | Beveiligd opslaggebied is geconfigureerd                                  |
| CURRENT BINARY            | "Samsung Official" = geen custom binaries actief                          |
| FRP LOCK                  | OFF = geen Google account-beveiliging (flash toegestaan)                  |
| OEM LOCK                  | OFF (U) = bootloader is ontgrendeld                                       |
| KG STATUS                 | CHECKING = normaal gedrag, verandert na netwerkverbinding                 |
| WARRANTY VOID             | 0x0 = geen knox-schade, toestel is schoon                                 |
| QUALCOMM SECUREBOOT       | ENABLE = vereist juiste vbmeta/recovery om te flashen                     |
| RP SWREV                  | Firmwareversiebeveiliging, geen probleem met juiste images                |
| SECURE DOWNLOAD           | ENABLE = vereist compatibele tool zoals Heimdall                          |
| GRDM STATUS               | NORMAL(D0000502) = geen blokkades                                          |

### Conclusie

> Bootloader is ontgrendeld  
> Secure Download vereist Heimdall  
> Flashing van vbmeta en recovery is toegestaan  

---

## 4. Verbinding testen met Heimdall

Controleer of Heimdall de tablet ziet:

```bash
heimdall detect
```

Uitvoer moet zijn:

```
Device detected
```

---

## 5. Recovery en vbmeta flashen

Zorg dat u zich in de map bevindt waar `vbmeta.img` en `recovery.img` zich bevinden.

```bash
sudo heimdall flash --VBMETA vbmeta.img --RECOVERY recovery.img --no-reboot
```

**Let op:** `--no-reboot` is cruciaal om te voorkomen dat Samsung de recovery meteen overschrijft.

### Verwachte uitvoer (samengevat):

- `VBMETA upload successful`
- `RECOVERY upload successful`
- `Session begun` / `Session ended`

---

## 6. Recovery opstarten met juiste toetsvolgorde

Na succesvol flashen:

1. Houd **Power + Volume Down** ingedrukt tot het scherm zwart wordt
2. Laat **Volume Down** los en houd direct **Power + Volume Up** ingedrukt
3. Houd vast tot het Samsung-logo verschijnt én verdwijnt
4. U komt in de **LineageOS Recovery**

---

## 7. LineageOS installeren via ADB sideload

1. In de recovery:
   - Kies **Apply update**
   - Kies **Apply from ADB**

2. Op uw pc:
   - Navigeer naar de map met de LineageOS-zip

```bash
adb sideload lineage-22.2-20250525-nightly-gta4l-signed.zip
```

### Verwachte melding:
```
Install complete with status: 0
```

---

## 8. Na de installatie

1. In de recovery:
   - Ga terug naar het hoofdmenu
   - Kies **Factory reset** → **Format data** → bevestigen

2. Daarna:
   - Kies **Reboot system now**

Uw tablet zal nu LineageOS voor het eerst opstarten 🎉

---

**Optioneel**: Installeer F-Droid en Aurora Store om toegang te krijgen tot apps zonder Google-account.

Einde van de handleiding. Veel plezier met uw vrije toestel!
