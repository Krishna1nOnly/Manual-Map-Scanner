# Manual Map Scanner

## Overview

Manual Map Scanner is a Windows-based security tool designed to detect and analyze suspicious threads and memory regions in running processes. It identifies potential manual mapped or shellcode threads by analyzing memory regions with high entropy and threads with start addresses in non-module memory. The tool is built for security researchers and system administrators to detect possible malicious activity.

**Author**: Krishna1nOnly

## Features

- **Process Scanning**: Scans all accessible processes for suspicious threads.
- **Memory Analysis**: Identifies executable memory regions not associated with loaded modules.
- **Entropy Calculation**: Computes entropy of memory regions to detect potential shellcode.
- **Thread Inspection**: Checks thread start addresses and states for anomalies.
- **PIN Authentication**: Requires a 4-digit PIN for access, with configurable attempt limits.
- **Thread Termination**: Allows termination of suspicious threads with user confirmation.
- **JSON Logging**: Logs scan results and errors in a JSON-like format for analysis.
- **Configurable Settings**: Supports a configuration file for customizing PIN, entropy threshold, and file paths.
- **Thread-Safe Logging**: Ensures reliable logging in a multi-threaded environment.

## Requirements

- **Operating System**: Windows 7 or later (tested on Windows 10/11).
- **Compiler**: Visual Studio 2019 or later with C++ support.
- **Dependencies**: Windows API libraries (`psapi.lib`).
- **Privileges**: Administrator privileges required for accessing process memory and terminating threads.

## Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/<your-username>/manual-map-scanner.git
   cd manual-map-scanner
   ```

2. **Open in Visual Studio**:
   - Open the `.sln` file or create a new project in Visual Studio.
   - Add `Manual Map Scanner.cpp` to the project.

3. **Link Libraries**:
   - Ensure `psapi.lib` is linked in the project settings:
     - Go to `Project > Properties > Linker > Input`.
     - Add `psapi.lib` to `Additional Dependencies`.

4. **Build the Project**:
   - Set the build configuration to `Release` or `Debug` (x86 or x64 based on your system).
   - Build the solution (`Ctrl+Shift+B`).

5. **Create Configuration File (Optional)**:
   - Create a `scanner_config.txt` file in the same directory as the executable with the following format:
     ```text
     pin=2099
     maxPinAttempts=3
     pinLength=4
     entropyThreshold=6.0
     logFile=scan_log.json
     outputFile=scan_results.json
     ```
   - The scanner will load these settings or use defaults if the file is absent.

## Usage

1. **Run the Scanner**:
   - Run the compiled executable as an administrator.
   - Enter the 4-digit PIN (default: `2099`) when prompted.
   - The scanner will list processes and report suspicious threads.

2. **Output**:
   - Suspicious threads are displayed with details (TID, Start Address, PID, Process Name, State, Priority).
   - Logs are saved to `scan_log.json` in JSON-like format.
   - Results can be exported to `scan_results.json` if chosen.

3. **Termination**:
   - After scanning, choose to terminate suspicious threads by entering `y` or `Y`.
   - Confirm export of results to `scan_results.json` if desired.

4. **Example Output**:
   ```
   ==============================================================
          Manual Map Scanner - Dev By Krishna1nOnly
   ==============================================================

   [+] Enter 4-digit PIN (3 attempts remaining): ****
   [+] PIN verification successful

   [+] Scanning for Manual Mapped or ShellCode Threads...

   notepad.exe
   No Suspicious Threads Found in notepad.exe.

   svchost.exe
   [!] Suspicious Thread ID: 1234, Start Address: 0x7FFA0000, State: Running, Priority: 8 in svchost.exe

   [+] Scan Complete. Summary:
       Total Suspicious Threads Found: 1

       Suspicious Threads:
       TID         Start Address       PID         Process Name                        State          Priority
       ----------------------------------------------------------------------------------------------------
       1234        0x7FFA0000          5678        svchost.exe                         Running        8

   [+] Terminate suspicious threads? (y/n): y
   [+] Terminated TID 1234 in svchost.exe
   [~] Termination complete. Terminated 1 of 1 threads.

   [+] Export results to scan_results.json? (y/n): y
   [+] Results exported to scan_results.json

   Press Enter to exit...
   ```

## Configuration

The `scanner_config.txt` file allows customization of the following parameters:
- `pin`: The 4-digit PIN for authentication (default: `2099`).
- `maxPinAttempts`: Maximum PIN entry attempts (default: `3`).
- `pinLength`: Length of the PIN (default: `4`).
- `entropyThreshold`: Minimum entropy for suspicious memory regions (default: `6.0`).
- `logFile`: Path to the log file (default: `scan_log.json`).
- `outputFile`: Path to the results file (default: `scan_results.json`).

## Security Notes

- **Run as Administrator**: Required for accessing process memory and terminating threads.
- **Log Integrity**: The scanner checks for log file tampering using a checksum.
- **Thread Termination**: Use with caution, as terminating threads may cause system instability.
- **PIN Protection**: Change the default PIN in the configuration file for security.

## Contributing

Contributions are welcome! Please follow these steps:
1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m 'Add your feature'`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a Pull Request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Disclaimer

This tool is for educational and research purposes only. Use it responsibly and only on systems you have permission to scan. The author is not responsible for any damage caused by misuse.

## Contact

For issues or suggestions, open an issue on GitHub or contact Krishna1nOnly at `<your-email>`.
