# Network Address and Host Allocation

## IP Address Structure

An IP address consists of two parts:
- **Network Portion**: Identifies the specific network.
- **Host Portion**: Identifies a specific device (host) within that network.

## Subnet Mask

The subnet mask defines how the IP address is divided into the network and host portions.

## Example Breakdown

For the given IP address and subnet mask:

- **IP Address**: 192.168.10.15
- **Subnet Mask**: 255.255.255.0

### Subnet Mask in Binary

- 255.255.255.0 in binary: `11111111.11111111.11111111.00000000`
- Here, the first 24 bits (`11111111.11111111.11111111`) represent the network portion, and the last 8 bits (`00000000`) represent the host portion.

### Network Address Calculation

- **IP Address in Binary**: `11000000.10101000.00001010.00001111` (192.168.10.15)
- **Subnet Mask in Binary**: `11111111.11111111.11111111.00000000` (255.255.255.0)
- **AND Operation Result**: `11000000.10101000.00001010.00000000` (192.168.10.0)

## Network Address

The result of the AND operation is the network address: `192.168.10.0`.

## Why the Network Address Cannot Be Assigned to a Device

The network address (`192.168.10.0`) is a special address that identifies the entire network. It is used by routers and other network devices to understand the network's identity. If this address were assigned to a device, it would create confusion, as network devices would not be able to differentiate between the network itself and a specific device within that network.

### Host Portion All 0s

- In the network address, the host portion is all zeros (`00000000`).
- This signifies that it is identifying the network as a whole rather than any specific host.

### Example

- **Network Address**: `192.168.10.0` (cannot be assigned to a device)
- **Usable Host Range**: 
  - From `192.168.10.1` (first usable IP address for devices)
  - To `192.168.10.254` (last usable IP address for devices)
- **Broadcast Address**: `192.168.10.255` (all host bits set to 1, used to communicate with all devices in the network simultaneously)

## Key Points

- **Network Address (`192.168.10.0`)**: Identifies the network.
- **Broadcast Address (`192.168.10.255`)**: Used for broadcasting messages to all devices in the network.
- **Usable IP Range**: Between the network and broadcast addresses (`192.168.10.1` to `192.168.10.254`).

Thus, the network address (`192.168.10.0`) serves a special purpose and cannot be allocated to individual devices.
