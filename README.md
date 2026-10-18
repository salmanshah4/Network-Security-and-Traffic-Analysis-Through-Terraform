# Azure Network Security and Traffic Analysis Lab Using Hub-and-Spoke, Terraform, and KQL

### Objective

This SOP explains how to build and validate an Azure hub-and-spoke network lab for security and traffic analysis. It covers provisioning resources with Terraform, testing firewall behavior with ping, capturing traffic on Linux VMs, and validating results in Log Analytics using KQL.

### Key Steps

### Link to Loom

<https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8>

**1. Build the Azure hub-and-spoke lab foundation** [0:07](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=7)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c0159c53-5590-4cb3-aca1-b6a9085856fb" />


- Create an Azure **hub-and-spoke topology** where traffic is routed through a central hub.
- Use the hub to centralize routing and security rules.
- Provision the lab infrastructure with **Terraform**.
- Include the required components: 
  - Virtual networks
  - Network security groups
  - Virtual machines
  - Firewall-related resources
- Confirm the environment is ready before testing traffic behavior.

 

**2. Prepare the virtual machines and monitoring tools** [0:41](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=41)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/27c36f80-1d44-44d5-a7cf-3aa3f25dfb3d" />


- Use **two Linux virtual machines** for the traffic test.
- Ensure both VMs are connected through the hub-and-spoke network.
- Set up **Log Analytics Workspace** for collecting and reviewing logs.
- Plan to analyze traffic using **KQL** queries.
- Verify that the VMs and logging workspace are accessible before starting the test.

 

**3. Test baseline traffic with the firewall rule set to allow** [1:07](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=67)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/250f65d2-e654-4b4b-843f-b63be29d60eb" />


- Remote into the first virtual machine.
- Confirm the firewall rule is currently set to **Allow**.
- Run a **ping test** between the VMs.
- Record the baseline result: 
  - Traffic should pass successfully
  - Packet loss should be **0%**
- Use this result as the normal/healthy network reference.

 

**4. Change the firewall rule to deny and validate packet loss** [1:42](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=102)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/06c7c53c-5779-4012-9c33-27c233dfb2b0" />


- Update the firewall rule from **Allow** to **Deny**.
- Apply the rule to the Azure firewall.
- Run the same **ping test** again.
- Confirm the expected failure condition: 
  - Traffic should be blocked
  - Packet loss should be **100%**
- Use this step to verify that the firewall rule is actively affecting network traffic.

 

**5. Restore the firewall rule to allow and re-test connectivity** [2:48](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=168)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2aade7fd-309e-48b0-8454-a6c7eba7d647" />


- Change the firewall rule back to **Allow**.
- Wait for the rule update to complete.
- Run the ping test again from the source VM.
- Confirm connectivity is restored: 
  - Traffic should pass normally
  - Packet loss should return to **0%**
- This confirms the environment can be toggled between blocked and allowed states for testing.

 

**6. Run a 60-second packet capture on the source VM** [3:32](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=212)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8070b069-ed9a-4e11-9c08-504fb6fdade0" />


- On **SpokeOne** (the first VM), start a **60-second packet capture**.
- Run the capture command quickly so the full capture window is used.
- Allow the capture to collect live network traffic during the test period.
- Wait for the capture to complete before moving to the next step.
- Use this capture to validate what traffic was actually observed on the VM.

 

**7. Check packet capture status and confirm results** [4:10](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=250)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/972dd3ff-e000-47da-bfe8-0e0bc9dc499c" />


- Run the command that checks the **capture status**.
- Review the live output from **tcpdump** or the capture tool being used.
- Confirm the capture completed successfully.
- Validate the result: 
  - Packets were received by the filter
  - **0 packets were dropped**
- Use this as evidence that the packet capture was successful and reliable.

 

**8. Query firewall and traffic logs in Log Analytics using KQL** [4:46](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=286)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e3a5ed3c-dd9f-4a6c-a486-fa5d04b6edea" />


- Open **Log Analytics Workspace** and go to **Logs**.
- Turn off **agent mode** if needed for easier querying.
- Run KQL queries to review firewall and traffic behavior.
- Focus on these query types: 
  - Full firewall rule log
  - Allowed-only results
  - Deny-only results
- Use the logs to correlate firewall actions with observed traffic behavior.

 

**9. Interpret the firewall rule log and traffic flow** [5:32](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=332)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/017c06a6-a2c2-42a5-9b3f-b142763b37f6" />

- Review the full firewall rule log output.
- Identify how the traffic moved through the environment: 
  - **Spoke 1** sends traffic through the **user-defined route**
  - Traffic passes through the **hub**
  - Traffic reaches **Spoke 2**
- Match the log entries to the firewall actions: 
  - Deny rule entries
  - Allow rule entries
  - Packet capture events
- Use the log to confirm the routing and security behavior matches the lab design.

 

**10. Review allowed and denied traffic results separately** [6:35](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=395)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bb05a464-6861-4606-9881-7210b4f91125" />


- Run a query that shows **allowed traffic only**.
- Confirm the allowed entries show traffic from **Spoke 1 to Spoke 2**.
- Run a query that shows **deny-only traffic**.
- Verify the deny count appears as expected.
- Use these filtered views to isolate specific firewall behaviors for reporting or troubleshooting.

 

**11. Summarize the lab outcome and document findings** [7:15](https://loom.com/share/8f89df103c104ed8a5bbf02870e11ea8?t=435)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8a1144b5-6b98-4645-b074-016deac84ab5" />


- Confirm the lab successfully demonstrated: 
  - Terraform-based Azure deployment
  - Firewall rule changes affecting traffic
  - Packet loss validation through ping
  - Packet capture validation on the VM
  - Log-based analysis through KQL
- Document the final results, including deny counts and observed traffic behavior.
- Save the findings as evidence of network security and traffic analysis in the lab.
- Close out the exercise and prepare for the next lab or review session.

### Cautionary Notes

- Ensure firewall rule changes are applied to the correct Azure firewall resource before testing.
- Wait for rule updates to fully propagate before running ping or capture tests.
- Packet capture commands may be time-sensitive; start them promptly to avoid missing traffic.
- Verify the correct VM is being used for each test so results are not mixed between spokes.
- If logs do not appear immediately in Log Analytics, allow time for ingestion before troubleshooting.

### Tips for Efficiency

- Use Terraform to recreate the lab consistently instead of configuring resources manually.
- Keep a baseline ping result before changing firewall rules so comparisons are easy.
- Run allow and deny tests in a repeatable sequence: allow → deny → allow.
- Save commonly used KQL queries for reuse during future investigations.
- Label resources clearly (hub, spoke VMs, firewall, workspace) to make log interpretation faster.
