# Python and R Scripts on the On-Premises Data Gateway (OPDG)

Decided to throw together a quick blog concerning the Microsoft On-Premises Data Gateway (OPDG) when talking Python and R Scripts.  
Here’s information on **On-Premises Data Gateway Standard Mode vs Personal Mode** specifically in the context of Python script support:

## Personal Mode

*   **Supports Python and R scripts** during refresh for Power BI datasets that include Power Query steps using these languages.
*   **Requirements:**
    *   Install the matching Python version locally.
    *   Configure executable paths in Power BI Desktop under `Options ▸ Python scripting`.
    *   Install required Python packages for the gateway user.
    *   Keep the machine powered on and signed in during refresh because personal mode runs as an interactive application, not as a Windows service.
*   **Scope:**
    *   Works only with Power BI (cannot be used for other services like Power Apps or Power Automate).
    *   Designed for single-user scenarios; cannot be shared with others. <https://learn.microsoft.com/en-us/power-bi/connect-data/service-gateway-personal>
*   **Best Practices for Setting Up Python with Personal Mode:**
    1.  **Install a Supported Python Version**
        *   Power BI Desktop and Personal Gateway typically support Python 3.x (64-bit).
        *   Avoid mixing 32-bit and 64-bit installations; the gateway requires 64-bit.
    2.  **Configure Python in Power BI Desktop**
        *   Go to `File ▸ Options ▸ Python scripting`.
        *   Set the Python home directory to the folder where `python.exe` resides (e.g., `C:\Python39`).
    3.  **Install Required Packages**
        *   Use `pip install` for libraries referenced in your Power Query scripts (e.g., `pandas`, `numpy`).
        *   Ensure packages are installed for the same Python environment configured in Power BI Desktop.
    4.  **Match Gateway and Desktop Environments**
        *   The Personal Gateway uses the same machine and user context as Desktop.
        *   Keep Python paths and packages consistent between Desktop and the environment where the gateway runs.
    5.  **Keep the Machine Online**
        *   Personal Mode runs as an interactive app, not a service.
        *   The machine must be powered on, signed in, and the gateway app running for scheduled refresh.
    6.  **Validate Scripts Before Publishing**
        *   Test your Python scripts in Desktop first.
        *   Avoid hard-coded local paths; use relative paths or parameterize file locations.
    7.  **Security Considerations**
        *   Scripts run under your user context; avoid sensitive operations or credentials in plain text.
        *   Ensure antivirus and endpoint protection allow Python execution.
*   **Limitations of Personal Mode:**
    *   **Single User Only:** Cannot be shared with other users or used for enterprise-wide data sources.
    *   **Power BI Only:** Does not support Power Apps, Power Automate, or other services.
    *   **No High Availability:** Cannot cluster or failover; if your machine is offline, refresh fails.
    *   **Requires Active Session:** Gateway must be running interactively; no background service execution.
    *   **Limited Scalability:** Not suitable for large datasets or multiple concurrent refreshes.
    *   **No Centralized Management:** Unlike Standard Mode, you cannot manage multiple gateways or apply enterprise policies.

## Standard Mode

*   **Does NOT support Python or R scripts for refresh.**
*   **Reason:**
    *   Standard mode runs as a Windows service under a gateway service account, which introduces security and permission constraints for executing arbitrary scripts.
*   **Scope:**
    *   Supports multiple users and multiple data sources across Power BI, Power Apps, Power Automate, and other services.
    *   Can be clustered for high availability and enterprise scenarios. <https://learn.microsoft.com/en-us/power-bi/connect-data/service-gateway-onprem>

## Key Operational Differences Table

| Feature           | Personal Mode                           | Standard Mode                       |
| ----------------- | --------------------------------------- | ----------------------------------- |
| Python/R Support  | ✅ Yes (Power BI only)                   | ❌ No                                |
| Runs As           | Interactive app (requires user sign-in) | Windows service (runs continuously) |
| Sharing           | Single user                             | Multi-user, multi-service           |
| High Availability | Not supported                           | Supported via clustering            |
| Use Case          | Individual report author                | Enterprise deployments              |

## Bottom Line

If your Power BI solution uses Python scripts in Power Query, you **must use Personal Mode** for scheduled refresh.  
Standard mode is ideal for enterprise-scale connectivity but cannot execute Python or R scripts.

## Sources - Documentation

*   <https://learn.microsoft.com/en-us/power-bi/connect-data/service-gateway-personal>
*   <https://community.fabric.microsoft.com/t5/Service/Solved-How-to-Install-amp-configure-Python-for-the-On-premi/td-p/123456>
