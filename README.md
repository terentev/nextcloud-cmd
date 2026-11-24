# nextcloudcmd для Ubuntu 26.04

Пакет для Ubuntu 26.04 (amd64) собран из исходников Nextcloud Desktop 4.0.6-1. Патч отключает HSTS в сетевом менеджере CLI при использовании `--trust`, позволяя клиенту принять недоверенный сертификат. Применяйте этот флаг только в доверенной сети: подмена сервера может привести к перехвату учётных данных.

- [Исходники пакета Ubuntu](source/nextcloud-desktop-4.0.6/)
- [Патч для `--trust`](patches/nextcloudcmd-trust.patch)
- [Пакет Ubuntu 26.04 (amd64)](packages/nextcloud-desktop-cmd_4.0.6-1%2Btrust1_amd64.deb)
- [Контрольная сумма SHA-256](packages/SHA256SUMS)

## Установка

```bash
cd packages
sha256sum -c SHA256SUMS
sudo apt install ./nextcloud-desktop-cmd_4.0.6-1+trust1_amd64.deb
```

Пакет использует штатную библиотеку Ubuntu `libnextcloudsync0t64` версии `4.0.6-1`.

## Сборка CLI из исходников

В Ubuntu 26.04 включите индексы исходных пакетов (`deb-src`) и установите сборочные зависимости через `sudo apt update` и `sudo apt build-dep nextcloud-desktop`. Затем из корня этого репозитория выполните:

```bash
cd source/nextcloud-desktop-4.0.6
patch -p1 < ../../patches/nextcloudcmd-trust.patch
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release \
  -DBUILD_GUI=OFF -DBUILD_SHELL_INTEGRATION=OFF -DBUILD_WITH_WEBENGINE=OFF \
  -DBUILD_UPDATER=OFF -DBUILD_TESTING=OFF -DUNIT_TESTING=OFF
cmake --build build --target nextcloudcmd --parallel
```

Бинарник появится в `build/bin/nextcloudcmd`.
