# z/OS Automation: Extracting LINKLIST with Python (ZOAU)

## Overview

This project demonstrates how to use Python with Z Open Automation Utilities (ZOAU) in USS to interact with z/OS system data. The script retrieves the system LINKLIST and writes it into a sequential dataset, then validates the output using a JCL job.

## Objective

* Extract system-level data (LINKLIST) from z/OS
* Transform the output into the required dataset format
* Write the data into a sequential dataset
* Validate correctness using a JCL job (CC0000)

---

## Technologies Used

* z/OS USS (Unix System Services)
* Python 3
* ZOAU (`zoautil_py`)
* JCL
* VS Code with Zowe Explorer

---

## Key Steps

### 1. Dataset Preparation

* Created or reused a sequential dataset:

  ```
  Z87690.COMPLETE
  ```
* Used Python (`datasets.create`) to ensure correct dataset type

---

### 2. Retrieve System LINKLIST

* Used ZOAU API:

  ```python
  zsystem.list_linklist()
  ```
* Returned system libraries such as:

  ```
  SYS1.MIGLIB
  SYS1.CSSLIB
  SYS1.LINKLIB
  ```

---

### 3. Data Transformation

* Converted Python list output into required z/OS dataset format:

  * One entry per line
  * Preserved single quotes
  * Removed list brackets and commas

---

### 4. Write to Dataset

* Used:

  ```python
  datasets.write(dsname, linklist_output, append=False)
  ```
* Ensured correct parameter order and formatting

---

### 5. Validation

* Submitted JCL job:

  ```
  CHKAUTO
  ```
* Achieved:

  ```
  CC 0000
  ```
* Confirmed dataset content met exact formatting requirements

* ## Code Snapshot

This is the Python script used to extract the z/OS LINKLIST and write it to a sequential dataset.

![members.py](docs/members_code.png)

---

## Challenges & Fixes

### Issue: Invalid dataset name errors

* Cause: Incorrect argument order in `datasets.write()`
* Fix: Corrected to `(dsname, data)`

### Issue: Validation failure (incorrect dataset content)

* Cause: Output formatting mismatch (missing quotes, brackets present)
* Fix: Reformatted output to match required structure

### Issue: CC0127 errors

* Cause: Dataset format and content mismatch with validation job
* Fix: Ensured sequential dataset + exact output formatting

---

## Key Takeaways

* z/OS workflows require **strict formatting precision**, not just correct logic
* ZOAU enables direct system interaction using Python
* Understanding dataset types (PDS vs sequential) is critical
* Debugging involved tracing both **data flow** and **system expectations**

---

## Outcome

Successfully built an end-to-end automation workflow:

USS (Python) → ZOAU API → z/OS Dataset → JCL Validation

---

## Next Steps

* Extend script to automate job submission using `jobs.submit()`
* Explore additional ZOAU modules for system monitoring and automation
