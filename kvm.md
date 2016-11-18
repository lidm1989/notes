# **安装**
    yum install libvirt-daemon-kvm virt-clone

# xml
    <domain type='kvm'>
        <name>test_ubuntu</name>
        <memory>1048576</memory>
        <currentMemory>1048576</currentMemory>
        <vcpu>8</vcpu>
        <os>
            <type arch='x86_64' machine='pc'>hvm</type>
            <boot dev='cdrom'/>
        </os>
        <features>
            <acpi/>
            <apic/>
            <pae/>
        </features>
        <clock offset='localtime'/>
        <on_poweroff>destroy</on_poweroff>
        <on_reboot>restart</on_reboot>
        <on_crash>destroy</on_crash>
        <devices>
            <emulator>/usr/libexec/qemu-kvm</emulator>
            <disk type='file' device='disk'>
            <driver name='qemu' type='qcow2'/>
            <source file='/var/lib/libvirt/images/test.qcow2'/>
            <target dev='hda' bus='ide'/>
            </disk>
            <disk type='file' device='cdrom'>
            <source file='/var/lib/libvirt/images/ubuntu.iso'/>
            <target dev='hdb' bus='ide'/>
            </disk>
        <interface type='bridge'>
            <source bridge='kvmbr0'/>
            <mac address="00:16:3e:5d:aa:a8"/>
        </interface>
        <input type='mouse' bus='ps2'/>
            <graphics type='vnc' port='-1' autoport='yes' listen = '0.0.0.0' keymap='en-us'/>
        </devices>
    </domain>

# 克隆
    virt-clone

# 网络
    virsh net-define <network.xml>
    virsh net-start default
    virsh net-list

    iptables -t nat -A POSTROUTING -o br0 -j MASQUERADE

# 创建
    qemu-img create -f qcow2 -o cluster_size=2M demo.qcow2 8G

    virsh define <domain>
    virsh start <domain>
    virsh shutdown <domain>
    virsh destroy <domain>
    virsh undefine <domain>
