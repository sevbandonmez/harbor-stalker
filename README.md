
# Harbor Stalker

A Python-based tool to scan Harbor container registries for potential vulnerabilities, specifically targeting **CVE-2022-46463** (Harbor Information Disclosure). This scanner identifies accessible repositories and generates URLs that may expose sensitive data without authentication.

[![Python Version](https://img.shields.io/badge/python-3.6+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![CVE-2022-46463](https://img.shields.io/badge/CVE-2022--46463-red.svg)](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-46463)

Inspired by the https://github.com/nu0l/CVE-2022-46463, the tool has been further enhanced.

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
  - [Command-Line Options](#command-line-options)
  - [Examples](#examples)
- [Output](#output)
- [Logging](#logging)
- [Contributing](#contributing)
- [Disclaimer](#disclaimer)
- [License](#license)
- [Author](#author)

## Overview
The **Harbor Registry Vulnerability Scanner** is designed to identify misconfigured Harbor registries that may expose repository information without proper authentication, as described in **CVE-2022-46463**. The tool searches for repositories using the Harbor API, generates potentially vulnerable URLs, and verifies their accessibility. It includes robust error handling, concurrent scanning, and comprehensive logging for reliable operation.
This tool is intended for **security researchers**, **penetration testers**, and **system administrators** to assess the security posture of Harbor deployments. Use it responsibly and only on systems you are authorized to test.

## Features
- **Concurrent Scanning**: Utilizes `ThreadPoolExecutor` for parallel repository searches, improving performance.
- **Robust Error Handling**: Gracefully handles network errors, JSON parsing issues, and invalid URLs.
- **Proxy Support**: Supports HTTP proxies (e.g., Burp Suite) for traffic inspection.
- **Comprehensive Logging**: Logs all activities to both console and file (`harbor_scan.log`) with colorized output.
- **Vulnerability Verification**: Optionally verifies generated URLs to confirm exploitability.
- **Flexible Input**: Scan a single URL or multiple targets from a file.
- **Detailed Reporting**: Generates timestamped reports with scan results.
- **Customizable**: Adjustable timeout, number of workers, and verbose logging options.

## Requirements
- **Python 3.6+**
- **Required Python libraries**:
  ```
  pip install requests urllib3
  ```

## Installation
**Clone the repository:**

``` git clone https://github.com/sevbandonmez/harbor-stalker.git && cd harbor-stalker ```
**Install dependencies:**
``` pip install -r requirements.txt ```
**Or directly:**
```pip install requests urllib3 ```

## Usage
**Run the script:**
```python3 harbor_scanner.py --help```
The scanner can be run with a single target URL or a file containing multiple URLs. It supports various command-line options for customization.

### Command-Line Options
```
python3 harbor_scanner.py [-h] [-u URL] [-f FILE] [--proxy PROXY] [--timeout TIMEOUT]
                         [--workers WORKERS] [--verify] [-v]

Options:
  -h, --help            Show this help message and exit
  -u URL, --url URL     Target URL (e.g., https://harbor.example.com)
  -f FILE, --file FILE  File containing target URLs (one per line)
  --proxy PROXY         HTTP proxy (e.g., http://127.0.0.1:8080)
  --timeout TIMEOUT     Request timeout in seconds (default: 10)
  --workers WORKERS     Number of concurrent workers (default: 5)
  --verify              Verify vulnerabilities by attempting to access URLs
  -v, --verbose         Enable verbose logging
```

### Examples
**Scan a single Harbor instance:**
```python3 harbor_scanner.py -u https://harbor.example.com```
**Scan multiple targets from a file:**
```python3 harbor_scanner.py -f targets.txt```
**Example targets.txt:**
```
https://harbor1.example.com
https://harbor2.example.com
```

**Verify vulnerabilities and enable verbose logging:**

```python3 harbor_scanner.py -u https://harbor.example.com --verify -v```

**Customize timeout and workers:**

```python3 harbor_scanner.py -f targets.txt --timeout 15 --workers 10```

### Output
**The scanner provides real-time console output with color-coded results:**
**Red: **Potentially vulnerable URLs found.
**Green:** No vulnerabilities detected.
**Yellow:** Target being scanned.
**Cyan:** Informational messages (e.g., scan completion).
A detailed report is saved as harbor_scan_report_<timestamp>.txt, including:
- Scan completion time
- Total targets scanned
- Total vulnerable URLs found
- List of vulnerable URLs per target

**Example console output:**
```
[!] Found 3 potentially vulnerable URLs:
Target: https://harbor.example.com
  • https://harbor.example.com/harbor/projects/1/repositories/nginx/artifacts-tab?publicAndNotLogged=yes [CONFIRMED]
  • https://harbor.example.com/harbor/projects/1/repositories/redis/artifacts-tab?publicAndNotLogged=yes [SAFE]
  • https://harbor.example.com/harbor/projects/2/repositories/mysql/artifacts-tab?publicAndNotLogged=yes [CONFIRMED]

[INFO] Scan completed. Check harbor_scan.log for detailed logs.
```

### Logging
All activities are logged to harbor_scan.log with timestamps and severity levels (INFO, WARNING, ERROR, DEBUG). Verbose mode (-v) enables DEBUG-level logging for detailed troubleshooting.

**Example log entry:**
```
2025-06-19 22:05:23,456 - INFO - Starting scan for: https://harbor.example.com
2025-06-19 22:05:24,789 - DEBUG - Found repository: {'project_id': 1, 'repository_name': 'nginx'}
2025-06-19 22:05:25,123 - WARNING - Search failed with status 429
```

## Contributing
Report issues or suggest improvements via the Issues page.

## Disclaimer
This tool is provided for educational and authorized testing purposes only. Unauthorized scanning of systems or networks is illegal and unethical. Ensure you have explicit permission from the system owner before using this tool. The author is not responsible for any misuse or damage caused by this software.

## License
This project is licensed under the MIT License. See the WTF file for details.

## Author
@sevbandonmez
For questions or feedback, open an issue or contact via social medias.


