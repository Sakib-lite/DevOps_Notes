# Subnetting Practice Question

## Questions:
1. What are the network address, broadcast address, and valid host addresses for the IP address `198.22.45.173/26`?
2. What is the subnet mask in dotted decimal notation?

## Explanation and Answers:

### Step 1: Determine the Subnet Mask
The given CIDR notation is /26. This means that the first 26 bits are the network portion, and the remaining bits are the host portion.

In binary, the subnet mask for /26 is:
`11111111.11111111.11111111.11000000`



Convert this binary subnet mask to dotted decimal notation:

- `11111111` = `255`
- `11111111` = `255`
- `11111111` = `255`
- `11000000` = `192`


So, the subnet mask in dotted decimal notation is:
    `255.255.255.192`


### Step 2: Determine the Network Address
The IP address given is `198.22.45.173`.

Convert the IP address to binary:
- `198` = `11000110`
- `22` = `00010110`
- `45` = `00101101`
- `173` = `10101101`


So, the IP address in binary is:
`11000110.00010110.00101101.10101101`


Perform a binary AND operation between the IP address and the subnet mask to find the network address:
- IP Address: `11000110.00010110.00101101.10101101`
- Subnet Mask: `11111111.11111111.11111111.11000000`
-----------------------------------
Network Address:`11000110.00010110.00101101.10000000`


Convert the network address back to decimal:
- `11000110` = `198`
- `00010110` = `22`
- `00101101` = `45`
- `10000000` = `128`


So, the network address is:
`198.22.45.128`


### Step 3: Determine the Broadcast Address
To find the broadcast address, set all the host bits to 1:
- Network Address: `11000110.00010110.00101101.10000000`
- Broadcast Address:`11000110.00010110.00101101.10111111`


Convert the broadcast address back to decimal:
- `11000110` = `198`
- `00010110` = `22`
- `00101101` = `45`
- `10111111` = `191`


So, the broadcast address is:
`198.22.45.191`


### Step 4: Determine the Valid Host Addresses
Valid host addresses are those between the network address and the broadcast address, excluding these two addresses.

So, the valid host addresses range from:
`198.22.45.129 `to `198.22.45.190`



## Summary of Answers:
- **Network Address**: `198.22.45.128`
- **Broadcast Address**: `198.22.45.191`
- **Valid Host Addresses**: `198.22.45.129` to `198.22.45.190`
- **Subnet Mask in Dotted Decimal Notation**: `255.255.255.192`