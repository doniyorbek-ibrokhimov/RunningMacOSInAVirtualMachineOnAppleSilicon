# Running macOS in a Virtual Machine on Apple Silicon

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-macOS-blue.svg)](https://developer.apple.com/macos/)
[![Xcode](https://img.shields.io/badge/Xcode-15.0+-blue.svg)](https://developer.apple.com/xcode/)

A comprehensive sample project demonstrating how to install and run macOS virtual machines (VMs) on Apple Silicon using Apple's Virtualization framework.

## Overview

This project provides both Swift and Objective-C implementations for creating and managing macOS virtual machines on Apple Silicon Macs. It showcases the power of Apple's Virtualization framework, enabling you to run complete macOS instances within your host system.

## Features

- ✅ **Full macOS VM Support**: Run complete macOS instances with hardware acceleration
- ✅ **Installation Tool**: Automated VM creation from IPSW files or downloaded restore images
- ✅ **Graphical Interface**: Native macOS app with virtual machine display
- ✅ **Hardware Integration**: Audio, networking, storage, and input device support
- ✅ **File Sharing**: Shared directories between host and guest systems
- ✅ **Save/Restore**: Pause and resume VM state (macOS 14.0+)
- ✅ **Dual Language Support**: Complete Swift and Objective-C implementations
- ✅ **Modern APIs**: Uses latest Virtualization framework features

## Requirements

- **Hardware**: Apple Silicon Mac (M1, M2, M3, or later)
- **Operating System**: macOS 13.0 or later
- **Development**: Xcode 15.0 or later
- **Memory**: At least 8GB RAM (16GB recommended for optimal performance)
- **Storage**: 50GB+ free space for VM disk images

## Project Structure

```
RunningMacOSInAVirtualMachineOnAppleSilicon/
├── Swift/                          # Swift implementation
│   ├── macOSVirtualMachineSampleApp/   # Main VM application
│   ├── InstallationTool/               # VM installation utility
│   └── Common/                         # Shared components
├── Objective-C/                    # Objective-C implementation
│   ├── macOSVirtualMachineSampleApp/   # Main VM application
│   ├── InstallationTool/               # VM installation utility
│   └── Common/                         # Shared components
├── Configuration/                  # Build configuration
└── macOSVirtualMachineSampleApp.xcodeproj/  # Xcode project
```

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/doniyorbek-ibrokhimov/RunningMacOSInAVirtualMachineOnAppleSilicon.git
cd RunningMacOSInAVirtualMachineOnAppleSilicon
```

### 2. Open in Xcode

```bash
open macOSVirtualMachineSampleApp.xcodeproj
```

### 3. Build the Project

Select either the Swift or Objective-C scheme and build:
- `macOSVirtualMachineSampleApp-Swift`
- `InstallationTool-Swift`
- `macOSVirtualMachineSampleApp-Objective-C`
- `InstallationTool-Objective-C`

## Usage

### Step 1: Install macOS in the Virtual Machine

Before running the main application, you need to install macOS using the Installation Tool:

#### Option A: Download Latest macOS Automatically
```bash
# Build and run InstallationTool without arguments to download the latest macOS
./InstallationTool
```

#### Option B: Use Specific IPSW File
```bash
# Build and run InstallationTool with path to IPSW file
./InstallationTool /path/to/your/macOS.ipsw
```

The installation process will:
1. Create a VM bundle directory
2. Download or use the specified macOS restore image
3. Set up virtual hardware configuration
4. Install macOS to the virtual disk

### Step 2: Run the Virtual Machine

After installation completes:

1. Build and run the `macOSVirtualMachineSampleApp` target
2. The app will automatically detect the installed VM and boot macOS
3. Complete the macOS setup process in the virtual machine

### Step 3: VM Management

- **Save State**: The VM automatically saves state when closing (macOS 14.0+)
- **File Sharing**: Access shared folders between host and guest
- **Full Screen**: Use the VM in full-screen mode for immersive experience

## Configuration

### Virtual Machine Specifications

The default VM configuration includes:
- **CPU Cores**: Half of available host cores (min: 1, respects framework limits)
- **Memory**: 16GB RAM (adjustable, respects framework limits)
- **Display**: 1920x1200 at 80 PPI
- **Network**: NAT networking with internet access
- **Audio**: Bidirectional audio support
- **Storage**: Virtio block device
- **Input**: Mac trackpad and keyboard support

### Customization

Modify `MacOSVirtualMachineConfigurationHelper.swift` (or `.m`) to adjust:
- Memory allocation in `computeMemorySize()`
- CPU count in `computeCPUCount()`
- Display resolution in `createGraphicsDeviceConfiguration()`
- Network settings in `createNetworkDeviceConfiguration()`

## File Locations

The project creates these directories in your home folder:

```
~/VMBundle/
├── AuxiliaryStorage.vmaux        # VM auxiliary storage
├── Disk.img                      # Virtual hard disk
├── HardwareModel                 # Hardware model data
├── MachineIdentifier            # Machine identifier
├── RestoreImage.ipsw            # macOS restore image (if downloaded)
└── SaveFile.vmsave              # VM save state (macOS 14.0+)

~/VMShared/                      # Shared folder between host and guest
```

## Troubleshooting

### Common Issues

1. **"Missing Virtual Machine Bundle" Error**
   - Run the InstallationTool first to create the VM bundle

2. **"Hardware model isn't supported" Error**
   - Ensure you're running on Apple Silicon hardware
   - Check that the hardware model data is valid

3. **Memory/CPU Allocation Errors**
   - Reduce memory size in configuration if host has limited RAM
   - Ensure adequate system resources are available

4. **Installation Hangs**
   - Verify stable internet connection for downloading restore images
   - Check available disk space (50GB+ recommended)

### Debug Mode

Enable verbose logging by adding debug flags in Xcode scheme settings.

## Technical Details

### Virtualization Framework Features Used

- `VZVirtualMachine`: Core virtual machine management
- `VZMacPlatformConfiguration`: Mac-specific platform setup
- `VZMacOSBootLoader`: macOS boot loader configuration
- `VZVirtioBlockDeviceConfiguration`: Storage device setup
- `VZMacGraphicsDeviceConfiguration`: Graphics acceleration
- `VZVirtioNetworkDeviceConfiguration`: Network connectivity
- `VZVirtioSoundDeviceConfiguration`: Audio support
- `VZVirtioFileSystemDeviceConfiguration`: File sharing

### Architecture Support

This project is designed specifically for Apple Silicon and includes:
- Compile-time architecture checks (`#if arch(arm64)`)
- Runtime hardware validation
- Apple Silicon-optimized VM configurations

## Contributing

Contributions are welcome! Please read the contribution guidelines and submit pull requests for any improvements.

### Development Guidelines

1. Maintain both Swift and Objective-C implementations
2. Follow Apple's coding conventions
3. Test on multiple Apple Silicon Mac models
4. Update documentation for any new features

## License

This project is licensed under the MIT License - see the [LICENSE.txt](LICENSE.txt) file for details.

## Related Resources

- [Apple Virtualization Framework Documentation](https://developer.apple.com/documentation/virtualization)
- [Running macOS in a virtual machine on Apple silicon](https://developer.apple.com/documentation/virtualization/Running-macOS-in-a-virtual-machine-on-Apple-silicon)
- [WWDC Sessions on Virtualization](https://developer.apple.com/videos/play/wwdc2022/10002/)

## System Requirements Summary

| Component | Requirement |
|-----------|-------------|
| Hardware | Apple Silicon Mac |
| macOS | 13.0+ |
| Xcode | 15.0+ |
| RAM | 8GB minimum, 16GB recommended |
| Storage | 50GB+ free space |
| Network | Internet connection for macOS download |

---

**Note**: This is sample code from Apple Inc. designed for educational and development purposes. Always ensure compliance with Apple's software licensing terms when using macOS in virtual machines.