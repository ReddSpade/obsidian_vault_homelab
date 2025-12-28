```txt
### COMPUTE

Proxmox
├── Dell Optiplex 3080 Micro
│   ├── CPU: Intel i5-10500T (6C/12T)
│   ├── RAM: 64Gb DDR4 SODIMM
│   ├── Stockage:
│   │   ├── Boot: 256Gb SSD 2.5'
│   │   └── Storage for VM: 2Tb NVMe
│   ├── Network:
│   │   ├── NIC01: 1Gb/s
│   │   └── NIC02: 2,5Gb/s
│   └── OS: Proxmox VE
│
├── Dell Optiplex 7080 Micro
│   ├── CPU: Intel i7-10700T (8C/16T)
│   ├── RAM: 64Gb DDR4 SODIMM
│   │   ├── Boot: 256Gb SSD 2.5'
│   │   └── Storage for VM: 2Tb NVMe
│   ├── Network:
│   │   ├── NIC01: 1Gb/s
│   │   └── NIC02: 2,5Gb/s
│   └── OS: Proxmox VE
│
└── Dell Optiplex 3080 Micro
    ├── CPU: Intel i5-10500T (6C/12T)
    ├── RAM: 64Gb DDR4 SODIMM
    │   ├── Boot: 256Gb SSD 2.5'
    │   └── Storage for VM: 2Tb NVMe
    ├── Network:
    │   ├── NIC01: 1Gb/s
    │   └── NIC02: 2,5Gb/s
    └── OS: Proxmox VE

Proxmox Backup Server
└── HP EliteDesk 800 G3
    ├── CPU: Intel i5-6500T (4C/4T)
    ├── RAM: 8Gb DDR4 SODIMM
    ├── Stockage:
    │   └── Storage for VM: 1Tb SSD 2.5'
    ├── Network:
    │   ├── NIC01: 1Gb/s
    │   └── NIC02: 2,5Gb/s
    └── OS: Proxmox Backup Server

### NETWORK

Router
└── Protectli Vault Pro VP2420
    ├── CPU: Intel Celeron J6412 (4C/4T)
    ├── RAM: 8Gb DDR4 SODIMM
    ├── Stockage:
    │   └── Storage for VM: 256Gb NVME
    ├── Network:
    │   ├── NIC01: 2,5Gb/s
    │   ├── NIC02: 2,5Gb/s
    │   ├── NIC03: 2,5Gb/s
    │   └── NIC04: 2,5Gb/s
    └── OS: OPNsense

Access Points
└── Unifi U7-Pro-XG

### STORAGE

NAS
├── Case: Fractal Node 804
├── CPU: AMD Ryzen 5 3400G (4C/8T)
├── RAM: 16GB DDR4
├── Storage:
│   ├── OS: NVMe
│   ├── Data:
│   │   └── 4x4Tb HDD WDRED PRO (RAID5)
│   └── Network:
│       └── NIC01: 2,5Gb/s
└── OS: TrueNAS Scale
```
