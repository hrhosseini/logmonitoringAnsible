# Ansible Playbook: Generate and Verify Logs in OpenSearch

## Overview
This Ansible playbook automates the process of generating a random log entry on one or more servers, sending the log to OpenSearch via Filebeat, and verifying its receipt. The playbook can be executed manually or through AWX.

## Features
- Generates a random log entry.
- Writes the log to `/var/log/sample.log`.
- Ensures Filebeat is running.
- Waits to allow log processing.
- Verifies OpenSearch connectivity.
- Searches for the generated log in OpenSearch.
- Validates log presence and content.

## Requirements
- Ansible installed on the control node.
- Filebeat installed and configured on target hosts.
- OpenSearch accessible from target hosts.
- User credentials for OpenSearch with sufficient permissions.

## Variables
The playbook uses the following variables:

| Variable | Description |
|----------|-------------|
| `hosts` | Target hosts where the log will be generated. |
| `log_file_path` | Path to the log file (default: `/var/log/sample.log`). |
| `opensearch_host` | OpenSearch endpoint (`{{ opensearchDomain }}:{{ opensearchPort }}`). |
| `opensearch_index` | OpenSearch index pattern (`filebeat*sample-*`). |
| `filebeat_config_path` | Path to Filebeat configuration (`/etc/filebeat/filebeat.yml`). |
| `opensearch_vm` | OpenSearch server to query logs. |
| `opensearch_admin_user` | OpenSearch admin username. |
| `opensearch_admin_password` | OpenSearch admin password. |
| `hex_chars` | Characters used for generating random log entries. |

## Execution

### Running Manually
Run the playbook using the following command:
```bash
ansible-playbook playbook.yml -e "hosts=your_target_hosts opensearchDomain=your_opensearch_host opensearchPort=your_port"
```

### Running in AWX
1. Upload the playbook to AWX.
2. Create a new Job Template.
3. Assign required credentials and inventory.
4. Run the job and monitor execution.

## Playbook Workflow
1. **Generate a Random Log Entry**
   - Creates an 8-character random string.
   - Logs the generated string to `/var/log/sample.log`.
2. **Ensure Filebeat is Running**
   - Starts the Filebeat service if not already running.
3. **Wait for Processing**
   - Introduces a delay to allow Filebeat to send logs.
4. **Verify OpenSearch Connectivity**
   - Checks OpenSearch cluster health.
5. **Search for Log Entry**
   - Queries OpenSearch to confirm log presence.
6. **Validate Log Content**
   - Asserts that the log is successfully stored.

## Troubleshooting
- **Filebeat logs not appearing in OpenSearch**
  - Check Filebeat logs (`journalctl -u filebeat -n 50`).
  - Verify OpenSearch credentials.
  - Ensure the correct index pattern is used.

- **OpenSearch connectivity issues**
  - Confirm OpenSearch is reachable from the host.
  - Check network/firewall rules.
  - Validate `opensearch_admin_user` and `opensearch_admin_password`.

## License
This project is licensed under the MIT License.

