# HTTPS SSL

This project focuses on configuring DNS subdomains and implementing SSL/HTTPS for secure web communications.

## Description

This project demonstrates understanding of DNS configuration, subdomain management, and secure web protocols. It includes scripts for auditing DNS records and configuring web infrastructure with proper domain routing.

## Learning Objectives

By completing this project, I have learned:

- What DNS is and its role in the internet infrastructure
- What subdomains are and how to configure them
- How to create and manage DNS A records
- The difference between HTTP and HTTPS
- What SSL/TLS certificates are and their purpose
- How to use `dig` to query DNS records
- How to parse command output using `awk`
- How to write Bash functions for code reusability

## Environment

- **Operating System:** Ubuntu 20.04 LTS
- **Tools Used:** dig, awk, bash
- **Domain:** atigbi.tech
- **DNS Provider:** .TECH Domains

## Project Structure

```
https_ssl/
├── README.md
└── 0-world_wide_web
```

## Tasks

### 0. World Wide Web
**File:** `0-world_wide_web`

A Bash script that audits DNS subdomain information for a given domain.

**Features:**
- Accepts domain name as mandatory parameter
- Accepts optional subdomain parameter
- Displays DNS record type and destination IP
- Uses `dig` for DNS queries and `awk` for parsing
- Implements Bash functions for code organization

**Usage:**

```bash
# Check all default subdomains
./0-world_wide_web atigbi.tech

# Check specific subdomain
./0-world_wide_web atigbi.tech web-01
```

**Output Format:**
```
The subdomain [SUB_DOMAIN] is a [RECORD_TYPE] record and points to [DESTINATION]
```

**DNS Configuration:**
- **www.atigbi.tech** → Load Balancer (44.202.82.65)
- **lb-01.atigbi.tech** → Load Balancer (44.202.82.65)
- **web-01.atigbi.tech** → Web Server 1 (52.87.245.215)
- **web-02.atigbi.tech** → Web Server 2 (54.165.247.234)

## Server Infrastructure

| Server | Hostname | IP Address | Role |
|--------|----------|------------|------|
| lb-01 | 6830-lb-01 | 44.202.82.65 | Load Balancer (HAProxy) |
| web-01 | 6830-web-01 | 52.87.245.215 | Web Server (Nginx) |
| web-02 | 6830-web-02 | 54.165.247.234 | Web Server (Nginx) |

## Technical Details

### DNS Record Types

**A Record (Address Record):**
- Maps a domain name to an IPv4 address
- Used for all subdomains in this project
- TTL: 3600 seconds (1 hour)

### Script Breakdown

The script uses:
1. **Bash Functions:** Encapsulates DNS query logic
2. **dig Command:** Queries DNS servers for record information
3. **awk Tool:** Parses dig output to extract record type and IP
4. **Conditional Logic:** Handles optional subdomain parameter

### How It Works

```bash
# Query DNS
dig subdomain.domain

# Extract ANSWER SECTION
grep -A1 'ANSWER SECTION:'

# Parse with awk
awk '{print $4}' # Record type (column 4)
awk '{print $5}' # IP address (column 5)
```

## Testing

### Verify DNS Configuration

```bash
# Test individual subdomains
dig www.atigbi.tech
dig lb-01.atigbi.tech
dig web-01.atigbi.tech
dig web-02.atigbi.tech

# Test script - all subdomains
./0-world_wide_web atigbi.tech

# Test script - specific subdomain
./0-world_wide_web atigbi.tech lb-01
```

### Expected Output

```
The subdomain www is a A record and points to 44.202.82.65
The subdomain lb-01 is a A record and points to 44.202.82.65
The subdomain web-01 is a A record and points to 52.87.245.215
The subdomain web-02 is a A record and points to 54.165.247.234
```

## Common DNS Commands

```bash
# Query A records
dig domain.com A

# Query specific subdomain
dig subdomain.domain.com

# Short answer format
dig +short domain.com

# Query specific DNS server
dig @8.8.8.8 domain.com

# Reverse DNS lookup
dig -x IP_ADDRESS
```

## Troubleshooting

### DNS Not Resolving

**Check DNS propagation:**
```bash
dig @8.8.8.8 subdomain.domain.com
dig @1.1.1.1 subdomain.domain.com
```

**Clear DNS cache (if needed):**
```bash
sudo systemd-resolve --flush-caches
```

**Wait for propagation:**
- DNS changes can take 15 minutes to 24 hours
- Most changes propagate within 1 hour

### Script Returns Empty Values

**Possible causes:**
1. DNS record not configured
2. DNS not yet propagated
3. Incorrect subdomain name
4. Typo in domain name

**Debug:**
```bash
# Test DNS manually
dig subdomain.domain.com | grep -A1 'ANSWER SECTION:'

# Check if subdomain exists
nslookup subdomain.domain.com
```

## Requirements

- Ubuntu 20.04 LTS
- bash
- dig (bind9-dnsutils package)
- awk (included in most systems)
- Domain name with DNS management access

## Installation

```bash
# Install dig if not available
sudo apt-get update
sudo apt-get install -y bind9-dnsutils

# Make script executable
chmod +x 0-world_wide_web
```

## Resources

- [DNS Basics](https://www.cloudflare.com/learning/dns/what-is-dns/)
- [Understanding A Records](https://support.dnsimple.com/articles/a-record/)
- [dig Command Tutorial](https://www.geeksforgeeks.org/dig-command-in-linux-with-examples/)
- [awk Guide](https://www.gnu.org/software/gawk/manual/gawk.html)

## Author

ALU Student - System Engineering & DevOps

## Repository

- **GitHub Repository:** alu-webstack
- **Directory:** https_ssl
- **Files:**
  - `0-world_wide_web` - DNS subdomain audit script
  - `README.md` - Project documentation