# saintclient-updates

Канал обновлений для **SAINT CLIENT** — лаунчера и мода. Здесь нет исходников: только два JSON-файла,
которые читают уже установленные клиенты, и релизы с бинарниками.

| Файл | Кто читает | Что описывает |
| --- | --- | --- |
| `latest.json` | лаунчер (`tauri-plugin-updater`) | версия лаунчера + подпись + ссылка на `-setup.exe` |
| `manifest.json` | лаунчер (`src-tauri/src/updater.rs`) | версия мода + sha256 + ссылка на `saintclient.jar` |

Бинарники лежат в [Releases](../../releases): теги `launcher-vX.Y.Z` и `mod-vX.Y.Z`.

## Как публиковать

Из папки лаунчера (`C:\Projects\SaintClientLauncher`):

```powershell
# новый мод
powershell -ExecutionPolicy Bypass -File scripts\publish-mod.ps1 -Version 0.2.0

# новая версия лаунчера (собирает и подписывает установщик)
powershell -ExecutionPolicy Bypass -File scripts\publish-launcher.ps1 -Version 0.2.0
```

Скрипт хеширует артефакт, переписывает JSON здесь, пушит его и печатает ссылку на создание релиза —
сам файл (`.jar` / `-setup.exe`) прикрепляется к релизу вручную.

## Подписи

`latest.json` содержит minisign-подпись установщика. Лаунчер проверяет её публичным ключом, вшитым
в `.exe`, — без приватного ключа (`\.keys\saintclient-updater.key`, в git не попадает) подсунуть
обновление невозможно, даже получив доступ к этому репозиторию. Ключ терять нельзя: без него
обновления для уже установленных клиентов выпустить не получится.
