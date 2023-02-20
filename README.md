# Virtualization:

## Настройка KVM на Linux:
```bash
Параметры ВМ -> Процессор -> Модуль виртуализации -> Включаем Виртуальный AMD-V/RVI        # включаем виртуализацию в VMware
$ egrep -c '(vmx|svm)' /proc/cpuinfo                                                       # проверяем поддержку виртуализации (0 - нету, 1 и больше - есть)
$ sudo apt install qemu qemu-kvm libvirt-daemon libvirt-clients bridge-utils virt-manager  # устанавливаем KVM и библиотеку libvirt
$ sudo gpasswd -a $USER libvirt                                                            # добавляем пользака в группу libvirt
$ sudo systemctl status libvirtd                                                           # проверяем, запустился ли сервис
$ kvm-ok                                                                                   # тоже проверка, должно показать: INFO: /dev/kvm exists KVM acceleration can be used
```

## Использование KVM:
```bash
$ ip link                            # посмотреть сетевой мост
$ sudo brctl show                    # посмотреть доступные мосты
$ sudo virsh -c qemu:///system list  # посмотреть список ВМ
$ reboot                             # перезагружаемся, чтобы настройки подтянулись
```

## Создание сети:
```xml
<network>
  <name>my-net</name>
  <uuid>585e140c-9559-4812-8a3c-ff4a4f037b6e</uuid>
  <forward mode="nat">
    <nat>
      <port start="1024" end="65535"/>
    </nat>
  </forward>
  <bridge name="vm-net0" stp="on" delay="0"/>
  <mac address="52:54:00:ab:dc:62"/>
  <domain name="my-net"/>
  <ip address="10.100.10.1" netmask="255.255.255.192">
    <tftp root="/srv/tftp"/>
    <dhcp>
      <range start="10.100.10.32" end="10.100.10.62"/>
      <bootp file="pxelinux.0" server="10.100.10.1"/>
    </dhcp>
  </ip>
</network>

```

## Настройка tftp:
```bash
$ cd /srv                                                     # идем в папку srv
$ sudo wget https://vcp.meganet.ru/dists/gb/tftp/tftp.tar.gz  # скачиваем архив
$ sudo tar -xvf tftp.tar.gz tftp/                             # распаковываем архив
```

## На Gentoo Linux вводим:
```bash
gdisk /dev/sda     # сканируем диски
Далее команда [o]  # удаляем все разделы (если таковые были).
Команда [n]        # создаем новый раздел:
- первый раздел grub (Enter, Enter, +1M, ef02)
- второй раздел swap (Enter, Enter, +1G, 8200)
- третий раздел root (Enter, Enter, +8G, Enter)
- четвертый раздел (Enter, Enter, Enter, Enter)
```
```bash
w       # сохраняем
reboot  # перезагружаемся
```
```bash
# Далее в меню загрузки выбираем Ubuntu Linux:
# Затем Install Ubuntu 20.04
# Русский → РФ → Нет → Russian → Russian → [Alt]+[Shift] → Вводим название компьютера: drbd01 → РФ → ru.archive.ubuntu.com → [Enter] → Tech → tech → 1 → 1 → да → Вручную
```
