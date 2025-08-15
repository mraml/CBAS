# CBAS - Compliance Benchmark Authoring Suite

**CBAS** is an enterprise-grade, in-browser tool designed to accelerate the creation of secure configuration baselines and hardening guides. It provides a comprehensive suite for security professionals and software vendors to analyze a product's source code against a set of security controls, generating actionable reports in standard formats like CSV and OSCAL.

### Dashboard

<img width="1787" height="938" alt="Screenshot 2025-08-15 at 4 00 02 PM" src="https://github.com/user-attachments/assets/f25015c4-abe1-4594-a5ed-7223bff17802" />

### Scan in Progress
<img width="1784" height="939" alt="Screenshot 2025-08-15 at 3 44 39 PM" src="https://github.com/user-attachments/assets/54c8035e-b5b7-488c-aafe-58bfaa91f14a" />

### Scan Results Preview
<img width="1777" height="939" alt="Screenshot 2025-08-15 at 4 03 09 PM" src="https://github.com/user-attachments/assets/4a6c24f7-3afc-4879-88f2-9fa4f9bff5c8" />

---

## Features

* **Professional UI**: A three-panel layout for intuitive navigation and analysis.
* **Robust Scanning Engine**: Ingests a zipped software repository and scans its contents against a custom heuristics library without freezing the UI.
* **Full Scan Control**: Manually initiate or stop scans at any time without losing your session data. The application provides real-time progress, including a progress bar, file counters, and an elapsed time clock.
* **Detailed Console Logging**: A toggleable console provides verbose, real-time feedback on the scan's progress, including successful file ingestions, scan status, and any errors encountered.
* **Resilient Scanning with Timeouts**:
    * **Per-File Timeout**: Skips any single file that takes too long to process, preventing the scanner from hanging on complex files.
    * **Global Timeout**: Ensures the entire scan terminates after a set duration as a final safeguard.
* **Actionable Reporting**:
    * **Categorized & Sorted Results**: Findings are automatically sorted and categorized by severity (**Critical, High, Moderate, Low, Informational**) to help prioritize remediation efforts.
    * **In-App Previews**: Toggle between scan results, a formatted CSV preview, and an OSCAL component definition preview directly in the UI.
    * **Multiple Export Formats**: Download the full scan report as a CSV file or a machine-readable OSCAL Component Definition (YAML).
    * **Exportable Logs**: Save the console output to a uniquely named text file for debugging and auditing.

---

## How to Use

1.  **Save the Application**: Download the `enterprise_hardening_studio_final.html` file and open it in a modern web browser (Chrome, Firefox, Edge).
2.  **Load Files**: Use the "Control Panel" on the left to upload the three required files in order:
    * **OSCAL Control Catalog**: A JSON file containing the security controls (e.g., `Catalog-800-53r5.json`).
    * **Heuristics Library**: Your custom-defined set of rules in JSON format (e.g., `lib-sec.json`).
    * **Software Repository**: The zipped source code of the product you want to scan.
3.  **Monitor Ingestion**: As each file is loaded, the status in the sidebar will update with the number of controls, rules, or files ingested. Check the console for detailed logs.
4.  **Run Scan**: Once all three files are loaded, the **Scan Repository** button will become active. Click it to begin the analysis.
5.  **Review Results**: As the scan runs, the main content area will display real-time progress. Upon completion (or if stopped), the results will be displayed, grouped by severity and then by the file in which they were found.
6.  **Preview and Export**: Use the toggle buttons at the top of the report to preview the CSV and OSCAL outputs. Use the buttons in the sidebar to download the final reports and the console log.

---

## Heuristics Library Format

The power of CBAS comes from its customizable heuristics library. This is a JSON file containing an array of rule objects. Each rule must follow this structure:

```json
{
  "heuristics": [
    {
      "family": "Category Name (e.g., Secrets Management)",
      "type": "content | filename | manual_review",
      "filename_pattern": "\\.(yml|py|conf)$",
      "pattern": "your_regular_expression_here",
      "severity": "Critical | High | Moderate | Low | Informational",
      "confidence": "High | Medium | Low",
      "controls": ["control-id-1", "control-id-2"],
      "justification": "Why this finding is a security concern.",
      "check": {
        "engine": "text",
        "template": "ACTION: Steps to verify the finding. Use `${file}` and `${line}`."
      },
      "fix": {
        "engine": "text",
        "template": "REMEDIATION: Steps to fix the issue."
      }
    }
  ]
}
