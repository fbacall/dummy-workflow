# dummy-workflow

## Idempotent Workflow RO-Crate Submission API

### Endpoint
https://dev.workflowhub.eu/workflows/submit

### Requirements

A URI (source URL) and version name are used together to determine whether to:
- Create a new workflow entry.
- Create a new version within an existing workflow entry.
- Or do nothing if the workflow and version have already been submitted.

#### Source URL
This is determined from (in descending order of precedence):
- `@id` of the Root Data Entity.
- `isBasedOn` of the Root Data Entity.
- `url` of the Root Data Entity.
- `url` of the Main Workflow.

#### Version
A `version` property on the Root Data Entity is required to name the submitted workflow/version.

### Examples
These examples assume the RO-Crate is in a file at path_to_your_ro.crate.zip, and you want to add it to a Team with ID 1234.

Via curl:
```
curl -X POST -H "Authorization: Token YOUR_TOKEN_HERE" \
             -F workflow[project_ids][]=1234 \
             -F ro_crate=@path_to_your_ro.crate.zip https://dev.workflowhub.eu/workflows/submit
```

Via requests in Python:
```
import requests
payload = { 'ro_crate': ('path_to_your_ro.crate.zip ', open('path_to_your_ro.crate.zip', 'rb')), 
            'workflow[project_ids][]': (None, '1234') }
headers = { 'authorization': 'Token YOUR_TOKEN_HERE' }

response = requests.post('https://dev.workflowhub.eu/workflows/submit', files=payload, headers=headers)
```
