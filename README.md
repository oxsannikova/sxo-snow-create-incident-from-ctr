# SecureX orchestrtration workflow: Create ServiceNow incident from SecureX threat response insident

This SecureX orchestration workflow looks for SecureX Threat Response new incidents and then creates a ServiceNow incident for any found.

Required Targets:
- CTR_For_Access_Token (default)
- CTR_API (default)
- ServiceNow

Required Account Keys:
- CTR_Credentials
- ServiceNow

Required Global Variables:
- Last Run Timestamp
