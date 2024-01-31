# Chehab MOSAAD (1A STRI)
## TP5 Administration Système Linux
## Vue 11 - Version du noyau et identification du paquet correspondant
```Powershell
09:25:14 etu@vm0 ~ → uname -a
Linux vm0 6.5.0-5-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.5.13-1 (2023-11-29) x86_64 GNU/Linux
09:21:16 etu@vm0 ~ → uname -r
6.5.0-5-amd64
09:21:24 etu@vm0 ~ → dpkg -l | grep linux-image
ii  linux-image-6.5.0-4-amd64       6.5.10-1                       amd64        Linux 6.5 for 64-bit PCs (signed)
ii  linux-image-6.5.0-5-amd64       6.5.13-1                       amd64        Linux 6.5 for 64-bit PCs (signed)
ii  linux-image-amd64               6.5.13-1                       amd64        Linux for 64-bit PCs (meta-package)
09:21:35 etu@vm0 ~ → journalctl -k
déc. 11 17:39:08 vm0 kernel: Linux version 6.5.0-5-amd64 (debian-kernel>
déc. 11 17:39:08 vm0 kernel: Command line: BOOT_IMAGE=/boot/vmlinuz-6.5>
déc. 11 17:39:08 vm0 kernel: BIOS-provided physical RAM map:
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x0000000000000000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x0000000000030000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x0000000000050000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x000000000009f000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x0000000000100000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x000000002e8ec000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x000000002e9ec000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x000000002eaec000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x000000002eb6c000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x000000002eb7e000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x000000002ebfe000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x000000003f000000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x00000000e0000000-0x00000>
déc. 11 17:39:08 vm0 kernel: BIOS-e820: [mem 0x00000000feffc000-0x00000>
déc. 11 17:39:08 vm0 kernel: NX (Execute Disable) protection: active
déc. 11 17:39:08 vm0 kernel: efi: EFI v2.7 by Debian distribution of ED>
déc. 11 17:39:08 vm0 kernel: efi: SMBIOS=0x2e9d4000 SMBIOS 3.0=0x2e9d20>
déc. 11 17:39:08 vm0 kernel: secureboot: Secure boot disabled
déc. 11 17:39:08 vm0 kernel: SMBIOS 3.0.0 present.
déc. 11 17:39:08 vm0 kernel: DMI: QEMU Standard PC (Q35 + ICH9, 2009), >
déc. 11 17:39:08 vm0 kernel: Hypervisor detected: KVM
déc. 11 17:39:08 vm0 kernel: kvm-clock: Using msrs 4b564d01 and 4b564d00
déc. 11 17:39:08 vm0 kernel: kvm-clock: using sched offset of 701763379>
déc. 11 17:39:08 vm0 kernel: clocksource: kvm-clock: mask: 0xffffffffff>
déc. 11 17:39:08 vm0 kernel: tsc: Detected 2394.374 MHz processor
déc. 11 17:39:08 vm0 kernel: e820: update [mem 0x00000000-0x00000fff] u>
déc. 11 17:39:08 vm0 kernel: e820: remove [mem 0x000a0000-0x000fffff] u>
déc. 11 17:39:08 vm0 kernel: last_pfn = 0x3f000 max_arch_pfn = 0x400000>
déc. 11 17:39:08 vm0 kernel: MTRR map: 4 entries (2 fixed + 2 variable;>
déc. 11 17:39:08 vm0 kernel: x86/PAT: Configuration [0-7]: WB  WC  UC- >
déc. 11 17:39:08 vm0 kernel: Using GB pages for direct mapping
déc. 11 17:39:08 vm0 kernel: RAMDISK: [mem 0x24497000-0x26414fff]
déc. 11 17:39:08 vm0 kernel: ACPI: Early table checksum verification di>
déc. 11 17:39:08 vm0 kernel: ACPI: RSDP 0x000000002EB7D014 000024 (v02 >
déc. 11 17:39:08 vm0 kernel: ACPI: XSDT 0x000000002EB7C0E8 000064 (v01 >
déc. 11 17:39:08 vm0 kernel: ACPI: FACP 0x000000002EB78000 0000F4 (v03 >

09:21:50 etu@vm0 ~ → kmod list
Module                  Size  Used by
tcp_diag               12288  0
inet_diag              28672  1 tcp_diag
cfg80211             1343488  0
rfkill                 40960  2 cfg80211
veth                   40960  0
btrfs                2023424  1
blake2b_generic        24576  0
xor                    20480  1 btrfs
raid6_pq              122880  1 btrfs
ebtable_filter         12288  0
ebtables               32768  1 ebtable_filter
ip6table_raw           12288  0
ip6table_mangle        12288  0
ip6table_nat           12288  0
ip6table_filter        12288  0
ip6_tables             28672  4 ip6table_filter,ip6table_raw,ip6table_nat,ip6table_mangle
iptable_raw            12288  0
iptable_mangle         12288  0
iptable_nat            12288  0
iptable_filter         12288  0
nf_tables             372736  0
unix_diag              12288  0
vhost_vsock            24576  0
vmw_vsock_virtio_transport_common    61440  1 vhost_vsock
vhost                  69632  1 vhost_vsock
vhost_iotlb            16384  1 vhost
vsock                  61440  4 vmw_vsock_virtio_transport_common,vhost_vsock
nfnetlink_cttimeout    16384  0
nfnetlink              20480  3 nfnetlink_cttimeout,nf_tables
openvswitch           217088  4
nsh                    12288  1 openvswitch
nf_conncount           24576  1 openvswitch
nf_nat                 65536  3 ip6table_nat,openvswitch,iptable_nat
nf_conntrack          212992  4 nf_nat,nfnetlink_cttimeout,openvswitch,nf_conncount
nf_defrag_ipv6         24576  1 nf_conntrack
nf_defrag_ipv4         12288  1 nf_conntrack
libcrc32c              12288  5 nf_conntrack,nf_nat,openvswitch,btrfs,nf_tables
binfmt_misc            28672  1
nls_ascii              12288  1
nls_cp437              16384  1
vfat                   20480  1
fat                   102400  1 vfat
intel_rapl_msr         20480  0
intel_rapl_common      36864  1 intel_rapl_msr
intel_uncore_frequency_common    16384  0
isst_if_common         24576  0
nfit                   81920  0
libnvdimm             245760  1 nfit
kvm_intel             413696  0
kvm                  1359872  1 kvm_intel
irqbypass              12288  1 kvm
ghash_clmulni_intel    16384  0
sha512_ssse3           53248  0
sha512_generic         16384  1 sha512_ssse3
sha256_ssse3           32768  0
sha1_ssse3             32768  0
ppdev                  24576  0
aesni_intel           360448  0
crypto_simd            16384  1 aesni_intel
cryptd                 28672  2 crypto_simd,ghash_clmulni_intel
rapl                   20480  0
snd_hda_codec_generic   114688  1
ledtrig_audio          12288  1 snd_hda_codec_generic
snd_hda_intel          61440  0
snd_intel_dspcfg       32768  1 snd_hda_intel
snd_intel_sdw_acpi     16384  1 snd_intel_dspcfg
snd_hda_codec         225280  2 snd_hda_codec_generic,snd_hda_intel
snd_hda_core          147456  3 snd_hda_codec_generic,snd_hda_intel,snd_hda_codec
snd_hwdep              20480  1 snd_hda_codec
snd_pcm               188416  3 snd_hda_intel,snd_hda_codec,snd_hda_core
qxl                    86016  0
snd_timer              53248  1 snd_pcm
pcspkr                 12288  0
drm_ttm_helper         12288  1 qxl
snd                   151552  6 snd_hda_codec_generic,snd_hwdep,snd_hda_intel,snd_hda_codec,snd_timer,snd_pcm
ttm                   106496  2 qxl,drm_ttm_helper
iTCO_wdt               12288  0
soundcore              16384  1 snd
intel_pmc_bxt          16384  1 iTCO_wdt
drm_kms_helper        270336  3 qxl
iTCO_vendor_support    12288  1 iTCO_wdt
virtio_balloon         32768  0
parport_pc             40960  0
parport                81920  2 parport_pc,ppdev
i6300esb               12288  0
watchdog               49152  2 iTCO_wdt,i6300esb
button                 24576  0
sg                     45056  0
joydev                 24576  0
evdev                  28672  2
serio_raw              16384  0
squashfs               81920  4
drm                   765952  5 drm_kms_helper,qxl,drm_ttm_helper,ttm
fuse                  208896  3
loop                   36864  10
dm_mod                221184  0
efi_pstore             12288  0
configfs               69632  1
efivarfs               24576  1
qemu_fw_cfg            20480  0
virtio_console         40960  0
virtio_rng             12288  0
ip_tables              28672  4 iptable_filter,iptable_raw,iptable_nat,iptable_mangle
x_tables               57344  11 ebtables,ip6table_filter,ip6table_raw,iptable_filter,ip6table_nat,ip6_tables,iptable_raw,ip_tables,iptable_nat,ip6table_mangle,iptable_mangle
autofs4                57344  2
ext4                 1130496  1
crc16                  12288  1 ext4
mbcache                16384  1 ext4
jbd2                  200704  1 ext4
crc32c_generic         12288  0
hid_generic            12288  0
usbhid                 73728  0
hid                   176128  2 usbhid,hid_generic
sr_mod                 28672  0
cdrom                  81920  1 sr_mod
virtio_net             81920  0
ahci                   49152  0
net_failover           24576  1 virtio_net
failover               12288  1 net_failover
libahci                61440  1 ahci
nvme                   57344  4
libata                475136  2 libahci,ahci
nvme_core             196608  5 nvme
uhci_hcd               61440  0
ehci_pci               16384  0
ehci_hcd              110592  1 ehci_pci
crc32_pclmul           12288  0
t10_pi                 20480  1 nvme_core
crc32c_intel           16384  4
psmouse               208896  0
scsi_mod              327680  3 libata,sg,sr_mod
usbcore               409600  4 ehci_pci,usbhid,ehci_hcd,uhci_hcd
i2c_i801               36864  0
i2c_smbus              16384  1 i2c_i801
crc64_rocksoft         16384  1 t10_pi
crc64                  16384  1 crc64_rocksoft
crc_t10dif             16384  1 t10_pi
scsi_common            16384  4 scsi_mod,libata,sg,sr_mod
virtio_pci             28672  0
virtio_pci_legacy_dev    16384  1 virtio_pci
crct10dif_generic      12288  0
lpc_ich                24576  0
virtio_pci_modern_dev    20480  1 virtio_pci
virtio                 20480  5 virtio_rng,virtio_console,virtio_balloon,virtio_pci,virtio_net
usb_common             20480  3 usbcore,ehci_hcd,uhci_hcd
crct10dif_pclmul       12288  1
crct10dif_common       12288  3 crct10dif_generic,crc_t10dif,crct10dif_pclmul
virtio_ring            53248  5 virtio_rng,virtio_console,virtio_balloon,virtio_pci,virtio_net
09:22:10 etu@vm0 ~ →
```
## Vue 11 - Affichage des 20 dernières lignes du journal du noyau
```Powershell
09:22:39 etu@vm0 ~ → sudo dmesg | tail -n 20
[sudo] Mot de passe de etu :
[588761.671266] audit: type=1400 audit(1702901505.466:779): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-19ad0d40ddcc02f451a3bb300490d8d22f899013ec7de9c3eeafbcae0ba3260f" pid=40813 comm="apparmor_parser"
[588761.830537] audit: type=1400 audit(1702901505.622:780): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-19ad0d40ddcc02f451a3bb300490d8d22f899013ec7de9c3eeafbcae0ba3260f" pid=40818 comm="apparmor_parser"
[588761.983476] audit: type=1400 audit(1702901505.778:781): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-19ad0d40ddcc02f451a3bb300490d8d22f899013ec7de9c3eeafbcae0ba3260f-rootfs" pid=40822 comm="apparmor_parser"
[588768.481916] audit: type=1400 audit(1702901512.274:782): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-19ad0d40ddcc02f451a3bb300490d8d22f899013ec7de9c3eeafbcae0ba3260f-rootfs" pid=40850 comm="apparmor_parser"
[675163.345045] audit: type=1400 audit(1702987906.911:783): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-5496aecd4862f77f50627b71638fcd5d3a3d9f1a40b076f5a36188494d1e77a5" pid=42729 comm="apparmor_parser"
[675163.537907] audit: type=1400 audit(1702987907.107:784): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-5496aecd4862f77f50627b71638fcd5d3a3d9f1a40b076f5a36188494d1e77a5" pid=42734 comm="apparmor_parser"
[675163.693371] audit: type=1400 audit(1702987907.259:785): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-5496aecd4862f77f50627b71638fcd5d3a3d9f1a40b076f5a36188494d1e77a5-rootfs" pid=42738 comm="apparmor_parser"
[675170.136137] audit: type=1400 audit(1702987913.703:786): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-5496aecd4862f77f50627b71638fcd5d3a3d9f1a40b076f5a36188494d1e77a5-rootfs" pid=42763 comm="apparmor_parser"
[761562.764590] audit: type=1400 audit(1703074306.095:787): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-1f8c54532eeb59fd6857d91d3ad3f9ab326a17c576f335882aa1b8fe9ca3f062" pid=44745 comm="apparmor_parser"
[761562.950340] audit: type=1400 audit(1703074306.283:788): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-1f8c54532eeb59fd6857d91d3ad3f9ab326a17c576f335882aa1b8fe9ca3f062" pid=44750 comm="apparmor_parser"
[761563.077629] audit: type=1400 audit(1703074306.411:789): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-1f8c54532eeb59fd6857d91d3ad3f9ab326a17c576f335882aa1b8fe9ca3f062-rootfs" pid=44754 comm="apparmor_parser"
[761569.364316] audit: type=1400 audit(1703074312.695:790): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-1f8c54532eeb59fd6857d91d3ad3f9ab326a17c576f335882aa1b8fe9ca3f062-rootfs" pid=44778 comm="apparmor_parser"
[847962.964334] audit: type=1400 audit(1703160706.054:791): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-5c530f58e10ad6a268beb73c1f8b5bec021ab5ab2b46e0a46d973d4117d3af72" pid=46818 comm="apparmor_parser"
[847963.096507] audit: type=1400 audit(1703160706.186:792): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-5c530f58e10ad6a268beb73c1f8b5bec021ab5ab2b46e0a46d973d4117d3af72" pid=46823 comm="apparmor_parser"
[847963.187087] audit: type=1400 audit(1703160706.278:793): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-5c530f58e10ad6a268beb73c1f8b5bec021ab5ab2b46e0a46d973d4117d3af72-rootfs" pid=46827 comm="apparmor_parser"
[847969.595209] audit: type=1400 audit(1703160712.686:794): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-5c530f58e10ad6a268beb73c1f8b5bec021ab5ab2b46e0a46d973d4117d3af72-rootfs" pid=46853 comm="apparmor_parser"
[934364.030845] audit: type=1400 audit(1703247106.906:795): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-32a67bcad24bbe918e200550b02aa8cddc5b013ed2109f3b63c688dc054d4e84" pid=48786 comm="apparmor_parser"
[934364.219189] audit: type=1400 audit(1703247107.094:796): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-32a67bcad24bbe918e200550b02aa8cddc5b013ed2109f3b63c688dc054d4e84" pid=48791 comm="apparmor_parser"
[934364.466697] audit: type=1400 audit(1703247107.342:797): apparmor="STATUS" operation="profile_load" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-32a67bcad24bbe918e200550b02aa8cddc5b013ed2109f3b63c688dc054d4e84-rootfs" pid=48795 comm="apparmor_parser"
[934370.887039] audit: type=1400 audit(1703247113.762:798): apparmor="STATUS" operation="profile_remove" profile="unconfined" name="lxd_archive-var-snap-lxd-common-lxd-storage-pools-default-images-32a67bcad24bbe918e200550b02aa8cddc5b013ed2109f3b63c688dc054d4e84-rootfs" pid=48822 comm="apparmor_parser"
```
## Vue 22 - État du service apache2
```Powershell
09:22:52 etu@vm0 ~ → systemctl status apache2
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; pres>
    Drop-In: /etc/systemd/system.control/apache2.service.d
             └─50-IOAccounting.conf
     Active: active (running) since Fri 2023-12-15 15:21:48 CET; 1 week>
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 34762 ExecStart=/usr/sbin/apachectl start (code=exited, st>
    Process: 49754 ExecReload=/usr/sbin/apachectl graceful (code=exited>
   Main PID: 34767 (apache2)
         IO: 23.3M read, 105.5M written
      Tasks: 55 (limit: 1079)
     Memory: 23.4M
        CPU: 51.922s
     CGroup: /system.slice/apache2.service
             ├─34767 /usr/sbin/apache2 -k start
             ├─49770 /usr/sbin/apache2 -k start
             └─49771 /usr/sbin/apache2 -k start

déc. 20 00:00:24 vm0 systemd[1]: Reloaded apache2.service - The Apache >
déc. 21 00:00:24 vm0 systemd[1]: Reloading apache2.service - The Apache>
déc. 21 00:00:24 vm0 apachectl[45609]: AH00558: apache2: Could not reli>
déc. 21 00:00:24 vm0 systemd[1]: Reloaded apache2.service - The Apache >
déc. 22 00:00:23 vm0 systemd[1]: Reloading apache2.service - The Apache>
déc. 22 00:00:23 vm0 apachectl[47681]: AH00558: apache2: Could not reli>
déc. 22 00:00:23 vm0 systemd[1]: Reloaded apache2.service - The Apache >
déc. 23 00:00:18 vm0 systemd[1]: Reloading apache2.service - The Apache>
déc. 23 00:00:18 vm0 apachectl[49757]: AH00558: apache2: Could not reli>
déc. 23 00:00:18 vm0 systemd[1]: Reloaded apache2.service - The Apache >

09:23:21 etu@vm0 ~ →
```
## État des services : systemd-networkd, systemd-resolved, systemd-timesyncd
```Powershell
09:23:21 etu@vm0 ~ → systemctl status systemd-networkd
○ systemd-networkd.service - Network Configuration
     Loaded: loaded (/lib/systemd/system/systemd-networkd.service; disa>
     Active: inactive (dead)
TriggeredBy: ○ systemd-networkd.socket
       Docs: man:systemd-networkd.service(8)
             man:org.freedesktop.network1(5)
   FD Store: 0 (limit: 512)

09:24:06 etu@vm0 ~ → systemctl status systemd-resolved
● systemd-resolved.service - Network Name Resolution
     Loaded: loaded (/lib/systemd/system/systemd-resolved.service; enabled; preset: enabled)
     Active: active (running) since Fri 2023-12-15 15:39:46 CET; 6s ago
       Docs: man:systemd-resolved.service(8)
             man:org.freedesktop.resolve1(5)
             https://www.freedesktop.org/wiki/Software/systemd/writing-network-configuration-managers
             https://www.freedesktop.org/wiki/Software/systemd/writing-resolver-clients
   Main PID: 3437 (systemd-resolve)
     Status: "Processing requests..."
      Tasks: 1 (limit: 1079)
     Memory: 2.6M
        CPU: 77ms
     CGroup: /system.slice/systemd-resolved.service
             └─3437 /lib/systemd/systemd-resolved

09:24:18 etu@vm0 ~ → systemctl status systemd-timesyncd
● systemd-timesyncd.service - Network Time Synchronization
     Loaded: loaded (/lib/systemd/system/systemd-timesyncd.service; ena>
     Active: active (running) since Fri 2023-12-15 15:39:48 CET; 1 week>
       Docs: man:systemd-timesyncd.service(8)
   Main PID: 515 (systemd-timesyn)
     Status: "Contacted time server 185.252.140.126:123 (0.debian.pool.>
      Tasks: 2 (limit: 1079)
     Memory: 756.0K
        CPU: 7.173s
     CGroup: /system.slice/systemd-timesyncd.service
             └─515 /lib/systemd/systemd-timesyncd

déc. 15 05:18:58 vm0 systemd-timesyncd[515]: Timed out waiting for repl>
déc. 15 05:19:08 vm0 systemd-timesyncd[515]: Timed out waiting for repl>
déc. 15 05:19:18 vm0 systemd-timesyncd[515]: Timed out waiting for repl>
déc. 15 05:53:37 vm0 systemd-timesyncd[515]: Contacted time server 193.>
déc. 15 08:45:04 vm0 systemd-timesyncd[515]: Timed out waiting for repl>
déc. 15 08:45:04 vm0 systemd-timesyncd[515]: Contacted time server 162.>
déc. 21 05:17:47 vm0 systemd-timesyncd[515]: Timed out waiting for repl>
déc. 21 05:17:57 vm0 systemd-timesyncd[515]: Timed out waiting for repl>
déc. 21 05:18:08 vm0 systemd-timesyncd[515]: Timed out waiting for repl>
déc. 21 05:53:07 vm0 systemd-timesyncd[515]: Contacted time server 185.>

09:24:50 etu@vm0 ~ → networkctl
systemd-networkd is not running, output might be incomplete.
IDX LINK         TYPE     OPERATIONAL SETUP
  1 lo           loopback -           unmanaged
  2 enp0s1       ether    -           unmanaged
  3 ovs-system   ether    -           unmanaged
  4 C-3P0        ether    -           unmanaged
 34 vethe8e5a1a8 ether    -           unmanaged

5 links listed.
09:25:24 etu@vm0 ~ → timedatectl
               Local time: sam. 2023-12-23 21:25:34 CET
           Universal time: sam. 2023-12-23 20:25:34 UTC
                 RTC time: sam. 2023-12-23 20:25:34
                Time zone: Europe/Paris (CET, +0100)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
09:25:34 etu@vm0 ~ →
```
# Fin