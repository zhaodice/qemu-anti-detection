# Qemu Anti Detection
A patch to hide qemu itself, bypass mhyprot,Anti Cheat Expert(ACE),Easy Anti Cheat(EAC),nProtect GameGuard(NP) / VMProtect,VProtect, Themida, Enigma Protector,Safegine Shielden

# Build and install qemu:
```
sudo apt-get update
sudo apt-get update  && sudo apt-get install -y     binutils-mingw-w64     binutils-mingw-w64-i686     binutils-mingw-w64-x86-64     build-essential     clang     g++-mingw-w64     g++-mingw-w64-i686     g++-mingw-w64-x86-64     gcc-mingw-w64     gcc-mingw-w64-i686     gcc-mingw-w64-x86-64     git     git-email     glib2.0-dev     gnutls-bin     libaio-dev     libbluetooth-dev     libbrlapi-dev     libbz2-dev     libcacard-dev     libcap-dev     libcap-ng-dev     libcurl4-gnutls-dev     libibverbs-dev     libiscsi-dev     libfdt-dev     libglib2.0-dev     libgtk-3-dev     libjpeg8-dev     liblzo2-dev     libncurses5-dev     libncursesw5-dev     libnfs-dev     libnuma-dev     libpam0g-dev     libpixman-1-dev     librbd-dev     librdmacm-dev     libsasl2-dev     libsdl1.2-dev     libsdl2-dev     libsdl2-image-dev     libseccomp-dev     libsnappy-dev     libssh2-1-dev     libusb-1.0-0-dev     libusb-dev     libvde-dev     libvdeplug-dev     libvirglrenderer-dev     libvte-2.91-dev     libxen-dev     libxml2-dev     libz-mingw-w64-dev     libzstd-dev     ninja-build     valgrind     win-iconv-mingw-w64-dev     xfslibs-dev     zlib1g-dev libspice-server-dev libusbredirparser-dev

git clone https://gitlab.com/qemu-project/qemu/ -b v7.0.0 --depth 1 --recursive
cd qemu
git apply qemu7.0.0.patch
cd ..
mkdir qemu_build
cd qemu_build
sudo ../qemu/configure --target-list=x86_64-softmmu,x86_64-linux-user --prefix=/usr
sudo make
sudo make install
```

# QEMU Config Reference Resources
```
<domain xmlns:qemu="http://libvirt.org/schemas/domain/qemu/1.0" type="kvm">
  <name>Entertainment</name>
  <uuid>ef533b31-5b16-4a54-8beb-57f881305131</uuid>
  <metadata>
    <libosinfo:libosinfo xmlns:libosinfo="http://libosinfo.org/xmlns/libvirt/domain/1.0">
      <libosinfo:os id="http://microsoft.com/win/10"/>
    </libosinfo:libosinfo>
  </metadata>
  <memory unit="KiB">8388608</memory>
  <currentMemory unit="KiB">8388608</currentMemory>
  <memoryBacking>
    <hugepages/>
  </memoryBacking>
  <vcpu placement="static">12</vcpu>
  <os firmware="efi">
    <type arch="x86_64" machine="pc-q35-7.0">hvm</type>
    <nvram>/passthough/OVMF_VARS.fd</nvram>
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
  <devices>
    <emulator>/usr/bin/qemu-system-x86_64</emulator>
    <disk type="file" device="disk">
      <driver name="qemu" type="qcow2"/>
      <source file="/media/user/SSD_VM/disk/data/ProgramData.qcow2"/>
      <target dev="sdd" bus="sata"/>
      <address type="drive" controller="0" bus="0" target="0" unit="3"/>
    </disk>
    <disk type="file" device="disk">
      <driver name="qemu" type="qcow2"/>
      <source file="/media/user/writable/VMData/data/Game.qcow2"/>
      <target dev="sdf" bus="sata"/>
      <address type="drive" controller="0" bus="0" target="0" unit="5"/>
    </disk>
    <disk type="file" device="disk">
      <driver name="qemu" type="qcow2"/>
      <source file="/media/user/SSD_VM/disk/data/extend.qcow2"/>
      <target dev="sdg" bus="sata"/>
      <address type="drive" controller="1" bus="0" target="0" unit="0"/>
    </disk>
    <disk type="file" device="disk">
      <driver name="qemu" type="qcow2"/>
      <source file="/media/user/SSD_VM/disk/win10.qcow2"/>
      <target dev="sdi" bus="sata"/>
      <boot order="1"/>
      <address type="drive" controller="1" bus="0" target="0" unit="2"/>
    </disk>
    <controller type="usb" index="0" model="qemu-xhci" ports="15">
      <address type="pci" domain="0x0000" bus="0x02" slot="0x00" function="0x0"/>
    </controller>
    <controller type="pci" index="0" model="pcie-root"/>
    <controller type="pci" index="1" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="1" port="0x8"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x0" multifunction="on"/>
    </controller>
    <controller type="pci" index="2" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="2" port="0x9"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x1"/>
    </controller>
    <controller type="pci" index="3" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="3" port="0xa"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x2"/>
    </controller>
    <controller type="pci" index="4" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="4" port="0xb"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x3"/>
    </controller>
    <controller type="pci" index="5" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="5" port="0xc"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x4"/>
    </controller>
    <controller type="pci" index="6" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="6" port="0xd"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x5"/>
    </controller>
    <controller type="pci" index="7" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="7" port="0xe"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x6"/>
    </controller>
    <controller type="pci" index="8" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="8" port="0xf"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x7"/>
    </controller>
    <controller type="pci" index="9" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="9" port="0x10"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x0" multifunction="on"/>
    </controller>
    <controller type="pci" index="10" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="10" port="0x11"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x1"/>
    </controller>
    <controller type="pci" index="11" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="11" port="0x12"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x2"/>
    </controller>
    <controller type="pci" index="12" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="12" port="0x13"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x3"/>
    </controller>
    <controller type="pci" index="13" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="13" port="0x14"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x4"/>
    </controller>
    <controller type="pci" index="14" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="14" port="0x15"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x5"/>
    </controller>
    <controller type="pci" index="15" model="pcie-root-port">
      <model name="pcie-root-port"/>
      <target chassis="15" port="0x16"/>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x6"/>
    </controller>
    <controller type="pci" index="16" model="pcie-to-pci-bridge">
      <model name="pcie-pci-bridge"/>
      <address type="pci" domain="0x0000" bus="0x01" slot="0x00" function="0x0"/>
    </controller>
    <controller type="sata" index="0">
      <address type="pci" domain="0x0000" bus="0x00" slot="0x1f" function="0x2"/>
    </controller>
    <controller type="sata" index="1">
      <address type="pci" domain="0x0000" bus="0x10" slot="0x02" function="0x0"/>
    </controller>
    <interface type="network">
      <mac address="10:54:00:1d:28:aa"/>
      <source network="default"/>
      <model type="rtl8139"/>
      <address type="pci" domain="0x0000" bus="0x10" slot="0x01" function="0x0"/>
    </interface>
    <input type="tablet" bus="usb">
      <address type="usb" bus="0" port="1"/>
    </input>
    <input type="mouse" bus="ps2"/>
    <input type="keyboard" bus="ps2"/>
    <input type="mouse" bus="usb">
      <address type="usb" bus="0" port="2"/>
    </input>
    <input type="keyboard" bus="usb">
      <address type="usb" bus="0" port="3"/>
    </input>
    <graphics type="spice">
      <listen type="none"/>
      <image compression="off"/>
      <gl enable="yes" rendernode="/dev/dri/by-path/pci-0000:00:02.0-render"/>
    </graphics>
    <sound model="ich9">
      <address type="pci" domain="0x0000" bus="0x00" slot="0x1b" function="0x0"/>
    </sound>
    <audio id="1" type="spice"/>
    <video>
      <model type="none"/>
    </video>
    <hostdev mode="subsystem" type="mdev" managed="yes" model="vfio-pci" display="off" ramfb="off">
      <source>
        <address uuid="af5972fb-5530-41a7-0000-fd836204445b"/>
      </source>
      <address type="pci" domain="0x0000" bus="0x04" slot="0x00" function="0x0"/>
    </hostdev>
    <hostdev mode="subsystem" type="pci" managed="yes">
      <source>
        <address domain="0x0000" bus="0x01" slot="0x00" function="0x0"/>
      </source>
      <address type="pci" domain="0x0000" bus="0x03" slot="0x00" function="0x0"/>
    </hostdev>
    <hostdev mode="subsystem" type="pci" managed="yes">
      <source>
        <address domain="0x0000" bus="0x01" slot="0x00" function="0x1"/>
      </source>
      <address type="pci" domain="0x0000" bus="0x05" slot="0x00" function="0x0"/>
    </hostdev>
    <redirdev bus="usb" type="spicevmc">
      <address type="usb" bus="0" port="4"/>
    </redirdev>
    <memballoon model="none"/>
  </devices>
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
    <qemu:arg value="host,family=6,model=158,stepping=2,model_id=Intel(R) Core(TM) i9-12900K CPU @ 2.60GHz,vmware-cpuid-freq=false,enforce=false,host-phys-bits=true,-hypervisor,+kvm_pv_unhalt,+kvm_pv_eoi,hv_spinlocks=0x1fff,hv_vapic,hv_time,hv_reset,hv_vpindex,hv_runtime,hv_relaxed,kvm=off,hv_vendor_id=intel"/>
    <qemu:arg value="-smp"/>
    <qemu:arg value="12,cores=12,threads=1,sockets=1"/>
    <qemu:arg value="-machine"/>
    <qemu:arg value="q35,kernel_irqchip=on"/>
    <qemu:arg value="-acpitable"/>
    <qemu:arg value="file=/passthough/ssdt1.dat"/>
    <qemu:env name="DISPLAY" value=":0.0"/>
    <qemu:env name="MESA_LOADER_DRIVER_OVERRIDE" value="i965"/>
  </qemu:commandline>
  <qemu:override>
    <qemu:device alias="hostdev0">
      <qemu:frontend>
        <qemu:property name="x-igd-opregion" type="bool" value="true"/>
        <qemu:property name="driver" type="string" value="vfio-pci-nohotplug"/>
        <qemu:property name="xres" type="unsigned" value="1920"/>
        <qemu:property name="yres" type="unsigned" value="1080"/>
        <qemu:property name="ramfb" type="bool" value="true"/>
        <qemu:property name="display" type="string" value="on"/>
        <qemu:property name="romfile" type="string" value="/passthough/vbios_gvt_uefi.rom"/>
      </qemu:frontend>
    </qemu:device>
    <qemu:device alias="hostdev1">
      <qemu:frontend>
        <qemu:property name="x-pci-vendor-id" type="unsigned" value="4318"/>
        <qemu:property name="x-pci-device-id" type="unsigned" value="8081"/>
        <qemu:property name="x-pci-sub-vendor-id" type="unsigned" value="5464"/>
        <qemu:property name="x-pci-sub-device-id" type="unsigned" value="34144"/>
      </qemu:frontend>
    </qemu:device>
  </qemu:override>
</domain>
```
![Screenshot_20220819_230305](https://user-images.githubusercontent.com/63996691/185649897-b7609626-ee6d-42b1-bc5e-4465cb41a19a.png)
