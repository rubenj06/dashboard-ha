# Hearth (fork)

Home Assistant add-on die het Hearth-dashboard bouwt uit de fork
[rubenj06/ha-hearth-rubenj06-fork](https://github.com/rubenj06/ha-hearth-rubenj06-fork), inclusief eigen aanpassingen.
Gebaseerd op [knowald/addon-ha-hearth](https://github.com/knowald/addon-ha-hearth) (MIT).

## Installeren

1. Ga in Home Assistant naar **Instellingen → Add-ons → Add-on Store**.
2. Kies **⋮ → Repositories** en voeg `https://github.com/rubenj06/ha-hearth-rubenj06-fork` toe.
3. Installeer **Hearth (fork)** en start de add-on.

Home Assistant bouwt de add-on bij installeren zelf; dat duurt een paar minuten. Hearth verschijnt
daarna in de zijbalk (Ingress). Voor een wandtablet kun je in de add-on-configuratie een poort
openzetten en **Home Assistant-URL voor directe toegang** invullen, bijvoorbeeld
`http://homeassistant.local:8123`.

De dashboardconfiguratie staat op het volume van de add-on en blijft bewaard bij updates.

## Een nieuwe versie uitrollen

De add-on bouwt altijd vanaf de `master`-branch van de fork. Home Assistant biedt pas een update aan
als `version` in `addon/config.yaml` omhoog gaat:

1. Zet de wijzigingen op `master`.
2. Verhoog `version` in `addon/config.yaml` (bijv. `0.3.0.1` → `0.3.0.2`; na een upstream-update naar
   0.4.0 wordt het `0.4.0.1`) en vul `CHANGELOG.md` aan.
3. Klik in Home Assistant op **Bijwerken**, of op **Controleren op updates** in de Add-on Store als de
   update nog niet zichtbaar is.

Mislukt het bouwen door te weinig geheugen, geef de Home Assistant-VM dan tijdelijk meer RAM.
