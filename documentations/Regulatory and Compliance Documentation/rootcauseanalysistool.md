# Explanation of Root Cause Analysis Tools

**Project Name:** Humanoid Robot Doctor (HRD)  
**Document Title:** Explanation of Root Cause Analysis Tools  
**Version:** 1.0  
**Date:** [Insert Date]  
**Prepared by:** [Your Name/Title]  
**Reviewed by:** [Reviewer’s Name/Title]  
**Approved by:** [Approver’s Name/Title]

---

### 1. Introduction

**Objective:**  
The objective of this document is to explain the various tools used in the root cause analysis (RCA) process. These tools help in identifying the underlying causes of problems or non-conformities within the Humanoid Robot Doctor (HRD) project, enabling effective resolution and prevention of recurrence.

**Scope:**  
This document covers the following RCA tools:
- 5 Whys Analysis
- Fishbone Diagram (Ishikawa Diagram)
- Pareto Analysis
- Failure Mode and Effects Analysis (FMEA)
- Fault Tree Analysis (FTA)

---

### 2. 5 Whys Analysis

**Description:**  
The 5 Whys technique involves asking "why" multiple times (usually five) to drill down into the cause-and-effect relationships underlying a problem. This simple but powerful tool helps identify the root cause of an issue by exploring successive layers of causation.

**Steps:**
1. Identify the problem.
2. Ask "Why did this happen?" and write down the answer.
3. If the answer doesn't reveal the root cause, ask "Why" again.
4. Repeat until the root cause is identified, usually by the fifth "Why."

**Example:**
- **Problem:** The HRD stopped working.
  - **Why?** The power supply failed.
  - **Why did the power supply fail?** It overheated.
  - **Why did it overheat?** The cooling fan stopped working.
  - **Why did the cooling fan stop working?** The fan motor burned out.
  - **Why did the fan motor burn out?** It was not maintained regularly.

**Usage:**  
Best for relatively simple problems where a straightforward cause-and-effect relationship can be established.

---

### 3. Fishbone Diagram (Ishikawa Diagram)

**Description:**  
The Fishbone Diagram, also known as the Ishikawa Diagram or Cause-and-Effect Diagram, visually maps out the potential causes of a problem. It categorizes causes into major areas, helping to systematically explore all possible root causes.

**Steps:**
1. Identify and clearly state the problem.
2. Draw a horizontal arrow pointing to the problem statement (the "head" of the fish).
3. Draw "bones" off the main arrow representing major categories of potential causes (e.g., People, Processes, Equipment, Materials, Environment, Management).
4. For each category, brainstorm potential causes and draw sub-bones for each cause.
5. Analyze the diagram to identify the most likely root causes.

**Example:**
- **Problem:** HRD data processing errors
  - **Categories:**
    - **People:** Lack of training
    - **Processes:** Inefficient data handling procedures
    - **Equipment:** Malfunctioning sensors
    - **Materials:** Poor quality data cables
    - **Environment:** High humidity affecting electronics
    - **Management:** Inadequate supervision

**Usage:**  
Ideal for complex problems where multiple potential causes need to be explored.

![output (16)](https://github.com/user-attachments/assets/cfada33f-f1ea-4f76-b07d-59df4da2d9be)
Here's the fishbone diagram for the Humanoid Robot Doctor (HRD) project. It visually maps out the potential causes of problems in six categories: People, Processes, Equipment, Materials, Environment, and Management. Each category lists specific potential causes that contribute to issues within the project. This tool helps in systematically identifying and addressing the root causes to ensure effective resolution and continuous improvement.

---

### 4. Pareto Analysis

**Description:**  
Pareto Analysis uses the 80/20 rule to identify the most significant factors contributing to a problem. It helps prioritize the issues that will have the most significant impact if addressed.

**Steps:**
1. List the problems or causes and their frequency or impact.
2. Rank the problems or causes from highest to lowest.
3. Calculate the cumulative percentage for each cause.
4. Identify the top 20% of causes that contribute to 80% of the problem.

**Example:**
- **Problem:** Frequent system downtimes
  - **Findings:**
    - 80% of downtimes caused by 20% of issues, such as software bugs and hardware failures.

**Usage:**  
Best for identifying and prioritizing the most critical issues to address for maximum impact.

### Key Points:

1. **Causes**: The x-axis lists the different causes of issues.
2. **Frequency**: The left y-axis and blue bars show the frequency of each cause.
3. **Cumulative Percentage**: The right y-axis and orange line show the cumulative percentage of the total occurrences.

The chart illustrates that the majority of issues are caused by a few significant factors, highlighting areas to prioritize for corrective actions. For instance, addressing software bugs and hardware failures could significantly reduce the overall number of issues, as these two causes contribute to a large portion of the problems.

![output (17)](https://github.com/user-attachments/assets/d09c61e2-f702-4e8a-b5bd-fcc58ac74d4d)
Here's the Pareto chart for the Humanoid Robot Doctor (HRD) project issues. 
This chart visually represents the frequency of different causes of issues and their cumulative impact. 

---

### 5. Failure Mode and Effects Analysis (FMEA)

**Description:**  
FMEA is a systematic method for evaluating processes to identify where and how they might fail and assessing the relative impact of different failures. It prioritizes issues based on their severity, occurrence, and detectability.

**Steps:**
1. Identify all potential failure modes for each component or process.
2. Assess the potential effects of each failure.
3. Determine the cause(s) of each failure mode.
4. Assign a severity rating, occurrence rating, and detectability rating to each failure mode.
5. Calculate the Risk Priority Number (RPN) for each failure mode (Severity x Occurrence x Detectability).
6. Prioritize the failure modes for corrective actions based on their RPN.

**Example:**
- **Component:** Power Supply
  - **Failure Mode:** Overheating
  - **Effect:** System shutdown
  - **Cause:** Cooling fan failure
  - **Severity:** 9 (on a scale of 1-10)
  - **Occurrence:** 4 (on a scale of 1-10)
  - **Detectability:** 3 (on a scale of 1-10)
  - **RPN:** 108 (Severity x Occurrence x Detectability)

**Usage:**  
Best for complex systems where multiple failure modes and their impacts need to be systematically analyzed.

---

### 6. Fault Tree Analysis (FTA)

**Description:**  
FTA is a top-down, deductive analytical method used to determine the various combinations of hardware and software failures and human errors that could result in a specific undesired event (top event).

**Steps:**
1. Define the top event (undesired event).
2. Identify all potential immediate causes of the top event.
3. For each cause, identify further causes leading to it.
4. Continue breaking down the causes into more basic events until reaching root causes.
5. Use logical gates (AND, OR) to represent the relationship between events.

**Example:**
- **Top Event:** HRD system failure
  - **Immediate Causes:**
    - Power supply failure (OR)
    - Software crash (OR)
    - Sensor malfunction (AND)

**Usage:**  
Ideal for complex systems where multiple interrelated factors contribute to a failure.

---

### 7. Conclusion

The Root Cause Analysis tools explained in this document provide a structured approach to identifying and addressing the underlying causes of problems within the HRD project. By selecting and applying the appropriate tool for each situation, the project team can effectively resolve issues and prevent their recurrence, supporting continuous improvement and project success.

**Signatures:**

**Project Manager:** ______________________  
**Quality Assurance Manager:** ______________________  
**Date:** ______________________  

---

This document provides a comprehensive explanation of various Root Cause Analysis tools, helping the HRD project team to systematically identify and address the underlying causes of problems and ensure continuous improvement.
