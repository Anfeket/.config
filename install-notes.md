## Bootloader

skusim systemd-boot

### TUF A15 quirks

napicu notebook, toto treba aby sa pc nezapinal hned po uspati do options:
`gpiolib_acpi.ignore_interrupt=AMDI0030:00@8,AMDI0030:00@16`
keby sa to posralo (aka ako debugovat):
1. `echo 1 > /sys/power/pm_debug_messages`
2. uspat (fuck around) a zistit (find out)
3. `dmesg | grep "GPIO.*active"` — tu budu ciselka
4. profit

## Secure boot

PK je owner
KEK su signeri
db, dbx su keys
```
sbctl create-keys
sbctl enroll-keys --microsoft
sbctl sign -s /boot/vmlinuz-linux
sbctl sign -s /boot/EFI/systemd/systemd-bootx64.efi
sbctl verify
```
`--microsoft` prida MS keys aby Windows isiel
`sbctl sign-all` resigne vsetko naraz, treba raz signut ale

## Suspend

pouzi `rtcwake` (bomba)

manualne:
`echo $(date -d "+5 minutes" '+%s') > /sys/class/rtc/rtc0/wakealarm`
vymazat:
`echo 0 > /sys/class/rtc/rtc0/wakealarm`
potom uspat s `systemctl suspend`

s `rtcwake`:
`rtcwake -s 300 -m mem`
`-m mem` je asi manualne uspatie, systemd neni spokojny (nepouzivat ig)
jasne, nic sa nestane:
```
rtcwake: wakeup from "mem" using /dev/rtc0 at Thu Mar 26 12:16:36 2026
rtcwake: write error    # huhhh
```
lepsie pouzit `-m no`

## Network

`iwd` je lepsi ako `wpa_supplicant`, treba otestovat
pouzi `avahi-browse -a` pre service discovery, `__minecraft.host` type shit

## Apps

### eza (ls replacement)
```bash
eza \
  -1  # oneline \
  -l  # long \
  -a  # all, show hidden files \
  --group-directories-first \
# --sort modified # not needed, looks default
```
