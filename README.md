# Bash Linux Server Health Check

A read-only Bash utility that reports basic host health information on Linux and macOS.

## Problem & Solution

- **Problem:** Checking uptime, memory, disk capacity, and service status manually takes repeated commands.
- **Approach:** The script gathers those values, flags high root-disk usage, and appends its output to a log.
- **Outcome:** A single command produces a quick, repeatable system-health snapshot without changing host settings.

## Screenshots

<table>
  <tr>
    <td align="center" valign="top">
      <img src="docs/screenshots/health-check-macos.png" alt="Health check running on macOS" width="420">
      <br><sub>Health check running on macOS</sub>
    </td>
    <td align="center" valign="top">
      <img src="docs/screenshots/health-check-ubuntu-run.png" alt="Health check running on Ubuntu" width="420">
      <br><sub>Health check running on Ubuntu</sub>
    </td>
  </tr>
  <tr>
    <td align="center" valign="top">
      <img src="docs/screenshots/health-check-ubuntu-tests.png" alt="Health check smoke tests passing" width="420">
      <br><sub>Health check smoke tests passing</sub>
    </td>
    <td></td>
  </tr>
</table>

## Architecture Diagram

![Bash Linux Server Health Check Workflow](docs/screenshots/health-check-workflow.drawio.png)

### Architecture Notes

The workflow diagram shows the main steps performed by the Bash Linux Server Health Check utility, from collecting system information to displaying and logging the results.

## Project Details

### Features


- Displays the current timestamp and hostname.
- Reports system uptime and load averages.
- Reports memory statistics.
- Displays root filesystem usage.
- Warns when root filesystem usage reaches 80% or above.
- Checks SSH service status using `systemctl` when available.
- Appends terminal output to a local log file.
- Includes smoke tests and Bash syntax checks.


### Requirements


- Bash
- Standard utilities: `date`, `hostname`, `uptime`, `df`, `awk`, and `tee`
- `free` for Linux memory reporting
- `systemctl` for Linux service checks

On macOS, the script uses `vm_stat` for memory reporting and skips the service check because `systemctl` is unavailable.


### Run the script


From the project directory:

```bash
chmod +x health-check.sh
./health-check.sh
```


### Run the smoke tests


```bash
chmod +x tests/test-health-check.sh
./tests/test-health-check.sh
```

These smoke tests verify that expected commands and sections exist in the script. They are not full functional tests.


### Check Bash syntax


```bash
bash -n health-check.sh
bash -n tests/test-health-check.sh
```


### Logging


Output is displayed in the terminal and appended to `logs/health-check.log`.

Generated log files are excluded from Git using `.gitignore`. Review logs before sharing because they can contain machine-specific information.


### Limitations


- The service check currently checks the SSH service.
- The disk warning threshold is fixed at 80%.
- Memory usage is displayed, but no memory warning threshold is implemented.
- Linux service behavior still needs to be verified on Ubuntu.
- The script reports system health; it does not automatically change settings or restart services.


### Project status


The smoke-test script and Bash syntax checks have been run on macOS and Ubuntu 24.04. The smoke tests check for expected commands and script sections; they do not exercise system behavior. The SSH service check requires a Linux system with `systemctl` and an SSH service, so verify it on the target distribution before relying on its result.

## Author

**Umar Arshad** · [GitHub](https://github.com/umararshad0)
