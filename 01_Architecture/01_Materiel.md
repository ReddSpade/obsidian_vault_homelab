```txt
### COMPUTE

Proxmox
├── Shuttle Slim XH510G
│   ├── CPU: Intel i5-10500T (6C/12T)
│   ├── RAM: 64Gb DDR4 SODIMM
│   ├── Stockage:
│   │   ├── Boot: 256Gb SSD 2.5'
│   │   └── Storage for VM: WD Blue SN5000 2Tb NVMe
│   ├── Network:
│   │   ├── NIC01: 1GbE
│   │   ├── NIC02: 10GbE
│   │   └── NIC03: 10GbE
│   │   ├── NIC02: 10GbE
│   │   └── NIC03: 10GbE
│   └── OS: Proxmox VE
│
├── Shuttle Slim XH510G
│   ├── CPU: Intel i7-10700T (8C/16T)
│   ├── RAM: 64Gb DDR4 SODIMM
│   │   ├── Boot: 256Gb SSD 2.5'
│   │   └── Storage for VM: WD Blue SN5000 2Tb NVMe
│   ├── Network:
│   │   ├── NIC01: 1GbE
│   │   ├── NIC02: 10GbE
│   │   └── NIC03: 10GbE
│   │   ├── NIC02: 10GbE
│   │   └── NIC03: 10GbE
│   └── OS: Proxmox VE
│
└── Shuttle Slim XH510G
    ├── CPU: Intel i5-10500T (6C/12T)
    ├── RAM: 64Gb DDR4 SODIMM
    │   ├── Boot: 256Gb SSD 2.5'
    │   └── Storage for VM: WD Blue SN5000 2Tb NVMe
    ├── Network:
    │   ├── NIC01: 1GbE
    │   ├── NIC02: 10GbE
    │   └── NIC03: 10GbE
    │   ├── NIC02: 10GbE
    │   └── NIC03: 10GbE
    └── OS: Proxmox VE

À déterminer
└── HP EliteDesk 800 G3
    ├── CPU: Intel i5-6500T (4C/4T)
    ├── RAM: 8Gb DDR4 SODIMM
    ├── Stockage:
    │   └── Storage for VM: 1Tb SSD 2.5'
    ├── Network:
    │   ├── NIC01: 1GbE
    │   └── NIC02: 2.5GbE
    └── OS: Proxmox Backup Server

### NETWORK

Router
└── Protectli Vault Pro VP2440
    ├── CPU: Intel N150 (4C/4T)
    ├── RAM: 8Gb DDR5 SODIMM
    ├── Stockage:
    │   └── Storage for VM: 256Gb NVME
    ├── Network:
    │   ├── NIC01: 2.5GbE
    │   ├── NIC02: 2.5GbE
    │   ├── NIC03: SFP+
    │   └── NIC04: SFP+
    └── OS: OPNsense

Access Points
└── Unifi U7-Pro-XG
    
Switch MikroTik CRS310-8G+2S+IN
└── Network
    ├── NIC01-8: 2,5GbE
	└── NIC09-10: SFP+
    
Switch MikroTik CRS309-1G-8S+IN
└── Network
    ├── NIC: PXE/BOOT
    └── NIC01-8: SFP+

### STORAGE

NAS
├── Case: Fractal Node 804
├── CPU: AMD Ryzen 5 3400G (4C/8T)
├── RAM: 16GB DDR4
├── Storage:
│   ├── OS: NVMe
│   └── Data:
│       └── 4x4Tb HDD WDRED PRO (RAID5)
├── Network:
│   ├── NIC01: 1GbE
│   └── NIC02: SFP+
└── OS: TrueNAS Scale

### OTHER

Serveur IA
├── Case: beQuiet! Silent Base 601
├── CPU: AMD Ryzen 5 5600X (6C/12T)
├── GPU: RTX 3090 Founders Edition
├── RAM: 32GB DDR4
├── Storage:
│   └── OS: NVMe 500Go
└── OS: Ubuntu Server LTS 24.04

UPS
└── CyberPower CP1350EPFCLCD
```

