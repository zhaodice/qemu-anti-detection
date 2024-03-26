## Other Project
For Proxmox VE(PVE) Anti Detection, see https://github.com/zhaodice/proxmox-ve-anti-detection

# QEMU Anti Detection
 | Type       | Engine | Bypass |
 |------------|--------|--------|
 | AntiCheat  | Anti Cheat Expert (ACE) | ☑️ |
 | AntiCheat  | Easy Anti Cheat (EAC) | ☑️ | 
 | AntiCheat  | Gepard Shield | ☑️ (But need to patch host kernel: https://github.com/WCharacter/RDTSC-KVM-Handler ) |
 | AntiCheat  | Mhyprot | ☑️ |
 | AntiCheat  | nProtect GameGuard (NP) | ☑️ | 
 | AntiCheat  | Roblox | ☑️ May Work with Hyper-V in the guest: https://github.com/zhaodice/qemu-anti-detection/issues/56 | 
 | AntiCheat  | Vanguard | ‼️(1: Incorrect function) | 
 | Encrypt    | Enigma Protector | ☑️ | 
 | Encrypt    | Safegine Shielden | ☑️ |
 | Encrypt    | Themida | ☑️ |
 | Encrypt    | VMProtect | ☑️ | 
 | Encrypt    | VProtect | ☑️ |       

‼️ There are games that cannot run under this environment but I am not sure whether QEMU has been detected, because the game doesn't report "Virtual machine detected" specifically. 
If you have any clue, feel free to tell me :)

### Flaws this patch does not fix in QEMU's source:
Those commands result in "No instance available" and could therefore EXPOSE THE VM. We do not yet know how to simulate this data.
```
wmic path Win32_Fan get *

wmic path Win32_CacheMemory get *

wmic path Win32_VoltageProbe get *

wmic path Win32_PerfFormattedData_Counters_ThermalZoneInformation get *

wmic path CIM_Memory get *

wmic path CIM_Sensor get *

wmic path CIM_NumericSensor get *

wmic path CIM_TemperatureSensor get *

wmic path CIM_VoltageSensor get *
```

# Build and install qemu:
```
sudo apt-get update
sudo apt-get update  && sudo apt-get install -y     binutils-mingw-w64     binutils-mingw-w64-i686     binutils-mingw-w64-x86-64     build-essential     clang     g++-mingw-w64     g++-mingw-w64-i686     g++-mingw-w64-x86-64     gcc-mingw-w64     gcc-mingw-w64-i686     gcc-mingw-w64-x86-64     git     git-email     gnutls-bin     libaio-dev     libbluetooth-dev     libbrlapi-dev     libbz2-dev     libcacard-dev     libcap-dev     libcap-ng-dev     libcurl4-gnutls-dev     libibverbs-dev     libiscsi-dev     libfdt-dev     libglib2.0-dev     libgtk-3-dev     libjpeg8-dev     liblzo2-dev     libncurses5-dev     libncursesw5-dev     libnfs-dev     libnuma-dev     libpam0g-dev     libpixman-1-dev     librbd-dev     librdmacm-dev     libsasl2-dev     libsdl1.2-dev     libsdl2-dev     libsdl2-image-dev     libseccomp-dev     libsnappy-dev     libssh2-1-dev     libusb-1.0-0-dev     libusb-dev     libvde-dev     libvdeplug-dev     libvirglrenderer-dev     libvte-2.91-dev     libxen-dev     libxml2-dev     libz-mingw-w64-dev     libzstd-dev     ninja-build     valgrind     win-iconv-mingw-w64-dev     xfslibs-dev     zlib1g-dev libspice-protocol-dev libspice-server-dev libusbredirparser-dev libusbredirparser1

git clone https://gitlab.com/qemu-project/qemu/ -b v7.0.0 --depth 1 --recursive
cd qemu
git apply qemu7.0.0.patch
cd ..
mkdir qemu_build
cd qemu_build
../qemu/configure --target-list=x86_64-softmmu,x86_64-linux-user --prefix=/usr
make -j $(nproc)
sudo make install
```
# DRIVER CODE 43 

see https://github.com/zhaodice/qemu-anti-detection/issues/48


# QEMU XML Config

Record your VIRTUAL MACHINE's uuid.
```
<domain xmlns:qemu="http://libvirt.org/schemas/domain/qemu/1.0" type="kvm">
  <name>Entertainment</name>
  <uuid>REPLACE YOUR UUID HERE!</uuid>
  <metadata>
    <libosinfo:libosinfo xmlns:libosinfo="http://libosinfo.org/xmlns/libvirt/domain/1.0">
      <libosinfo:os id="http://microsoft.com/win/10"/>
    </libosinfo:libosinfo>
  </metadata>
  <memory unit="KiB">1548288</memory>
  <currentMemory unit="KiB">1548288</currentMemory>
  <memoryBacking>
    <source type="memfd"/>
    <access mode="shared"/>
  </memoryBacking>
  <vcpu placement="static">12</vcpu>
  <os firmware="efi">
    <type arch="x86_64" machine="pc-q35-7.0">hvm</type>
    <loader/>
  </os>
  <features>
    <acpi/>
    <apic/>
    <hyperv mode="custom">
      <relaxed state="on"/>
      <vapic state="on"/>
      <spinlocks state="on" retries="8191"/>
      <vendor_id state="on" value="GenuineIntel"/>
    </hyperv>
    <kvm>
      <hidden state="on"/>
    </kvm>
    <vmport state="off"/>
    <smm state="on"/>
    <ioapic driver="kvm"/>
  </features>
  <cpu mode="host-model" check="partial"/>
  <clock offset="localtime">
    <timer name="rtc" tickpolicy="catchup"/>
    <timer name="pit" tickpolicy="delay"/>
    <timer name="hpet" present="no"/>
    <timer name="hypervclock" present="yes"/>
  </clock>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>destroy</on_crash>
  <pm>
    <suspend-to-mem enabled="no"/>
    <suspend-to-disk enabled="no"/>
  </pm>
  <qemu:commandline>
    <qemu:arg value="-smbios"/>
    <qemu:arg value="type=0,version=UX305UA.201"/>
    <qemu:arg value="-smbios"/>
    <qemu:arg value="type=1,manufacturer=ASUS,product=UX305UA,version=2021.1"/>
    <qemu:arg value="-smbios"/>
    <qemu:arg value="type=2,manufacturer=Intel,version=2021.5,product=Intel i9-12900K"/>
    <qemu:arg value="-smbios"/>
    <qemu:arg value="type=3,manufacturer=XBZJ"/>
    <qemu:arg value="-smbios"/>
    <qemu:arg value="type=17,manufacturer=KINGSTON,loc_pfx=DDR5,speed=4800,serial=000000,part=0000"/>
    <qemu:arg value="-smbios"/>
    <qemu:arg value="type=4,manufacturer=Intel,max-speed=4800,current-speed=4800"/>
    <qemu:arg value="-cpu"/>
    <qemu:arg value="host,family=6,model=158,stepping=2,model_id=Intel(R) Core(TM) i9-12900K CPU @ 2.60GHz,vmware-cpuid-freq=false,enforce=false,host-phys-bits=true,hypervisor=off"/>
    <qemu:arg value="-machine"/>
    <qemu:arg value="q35,kernel_irqchip=on"/>
  </qemu:commandline>
</domain>
```
![Screenshot_20220819_230305](https://user-images.githubusercontent.com/63996691/185649897-b7609626-ee6d-42b1-bc5e-4465cb41a19a.png)
