# Network Subnetting Plan

## Given Network
-   [
![Screenshot (14)](https://github.com/Sakib-lite/DevOps_Notes/assets/77607002/f29b8185-f7dd-49ca-bab6-384ef421537c)
](url)
- Network: `200.15.10.0/24`
- Total IP Addresses: 256 (from `200.15.10.0` to `200.15.10.255`)

## Requirements
- Sales (New York): 14 hosts
- Sales (Boston): 7 hosts
- Engineering (New York): 28 hosts
- Engineering (Boston): 28 hosts
- Point-to-Point Link: 2 hosts

## Subnet Calculation Process
We'll use Variable Length Subnet Masking (VLSM) to efficiently allocate subnets based on host requirements. We'll start with the largest subnet requirement and proceed to the smallest.

1. **Engineering (New York)**
   - Hosts Required: 28
   - Subnet Needed: `2^(5+1) = 32` IP addresses (5 host bits, plus 1 bit for network)
   - Subnet Mask: `/27 (255.255.255.224)`
   - Subnet: `200.15.10.0/27`
   - Range: `200.15.10.0 - 200.15.10.31`

2. **Engineering (Boston)**
   - Hosts Required: 28
   - Subnet Needed: `32` IP addresses
   - Subnet Mask: `/27 (255.255.255.224)`
   - Subnet: `200.15.10.32/27`
   - Range: `200.15.10.32 - 200.15.10.63`

3. **Sales (New York)**
   - Hosts Required: 14
   - Subnet Needed: `2^(4+1) = 16` IP addresses (4 host bits, plus 1 bit for network)
   - Subnet Mask: `/28 (255.255.255.240)`
   - Subnet: `200.15.10.64/28`
   - Range: `200.15.10.64 - 200.15.10.79`

4. **Sales (Boston)**
   - Hosts Required: 7
   - Subnet Needed: `2^(3+1) = 8` IP addresses (3 host bits, plus 1 bit for network)
   - Subnet Mask: `/29 (255.255.255.248)`
   - Subnet: `200.15.10.80/29`
   - Range: `200.15.10.80 - 200.15.10.87`

5. **Point-to-Point Link (between New York and Boston)**
   - Hosts Required: 2
   - Subnet Needed: `2^(2+1) = 4` IP addresses (2 host bits, plus 1 bit for network)
   - Subnet Mask: `/30 (255.255.255.252)`
   - Subnet: `200.15.10.88/30`
   - Range: `200.15.10.88 - 200.15.10.91`

## Detailed Explanation for Each Subnet

### 1. Engineering (New York)
- Subnet: `200.15.10.0/27`
- Binary Subnet Mask: `11111111.11111111.11111111.11100000`
- Network Address: `200.15.10.0`
- Broadcast Address: `200.15.10.31`
- Usable IP Range: `200.15.10.1 - 200.15.10.30`

### 2. Engineering (Boston)
- Subnet: `200.15.10.32/27`
- Binary Subnet Mask: `11111111.11111111.11111111.11100000`
- Network Address: `200.15.10.32`
- Broadcast Address: `200.15.10.63`
- Usable IP Range: `200.15.10.33 - 200.15.10.62`

### 3. Sales (New York)
- Subnet: `200.15.10.64/28`
- Binary Subnet Mask: `11111111.11111111.11111111.11110000`
- Network Address: `200.15.10.64`
- Broadcast Address: `200.15.10.79`
- Usable IP Range: `200.15.10.65 - 200.15.10.78`

### 4. Sales (Boston)
- Subnet: `200.15.10.80/29`
- Binary Subnet Mask: `11111111.11111111.11111111.11111000`
- Network Address: `200.15.10.80`
- Broadcast Address: `200.15.10.87`
- Usable IP Range: `200.15.10.81 - 200.15.10.86`

### 5. Point-to-Point Link (New York - Boston)
- Subnet: `200.15.10.88/30`
- Binary Subnet Mask: `11111111.11111111.11111111.11111100`
- Network Address: `200.15.10.88`
- Broadcast Address: `200.15.10.91`
- Usable IP Range: `200.15.10.89 - 200.15.10.90`

## Final Subnet Allocation
- **Engineering (New York)**: `200.15.10.0/27` - `200.15.10.0 - 200.15.10.31`
- **Engineering (Boston)**: `200.15.10.32/27` - `200.15.10.32 - 200.15.10.63`
- **Sales (New York)**: `200.15.10.64/28` - `200.15.10.64 - 200.15.10.79`
- **Sales (Boston)**: `200.15.10.80/29` - `200.15.10.80 - 200.15.10.87`
- **Point-to-Point Link**: `200.15.10.88/30` - `200.15.10.88 - 200.15.10.91`

By following this method, we've effectively subdivided the larger network into smaller subnets that meet the specific needs of each department and the point-to-point link while ensuring efficient use of IP addresses.
