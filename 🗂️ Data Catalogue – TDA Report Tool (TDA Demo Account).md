

**Environment:** `TDA Demo`  
**Status:** `In Test` *(previously: In Design)*  
**Document Owner:** [[🧘🏼‍♀️Mariateresa Linsalata]]
**Last Updated:** 2025-04-18

---

## 📁 Main Table

The `Main Table` (code: `TDAREP`) defines the high-level structure of the TDA Report for IVDR Product Assessors.  
Below are the current fields defined within this table (into Ariadne - Administration - Tables).

---

###  1. `Label`  
- **Data Type:** `None`  
- **Code:** `NBINF`  
- **Description:** Notified Body: SGS Belgium NV, Noorderlaan 87, BE-2030 Antwerpen, Belgium Notified Body No: 1639
---

### 2. `Panel: Administrative Particulars - notified body, manufacturer and product reference
- Data type: `Panel`  
- Code: `PAN`  
- Description: Administrative Particulars - notified body, manufacturer and product reference 
- **Note:** the panel groups multiple related fields. This is a *design-level* container and does not hold data itself, but includes the following "child fields":
#### ▸ Field: `In-vitro diagnostic device name, model and type`  
- **Data Type:** `String`  
- **Code:** `IVDNMT`  
- **Input from User**: Yes
- **Mandatory:** Yes
- **Description:** In-vitro diagnostic device name, model and type

#### ▸ Field: `Basic UDI-DI(s)`
- **Data Type:** `String`
- **Code:** `UDI-DI`
- **Input from User:** Yes
- **Mandatory:** No
- **Description:** Basic UDI-DI(s) (if available). The unique device identifier for in-vitro diagnostic devices, provided by the manufacturer.

#### ▸ Field: `Certificate Number`
- **Data Type:** `String`
- **Code:** `CERTN`
- **Input from User:** Yes
- **Mandatory:** No
- **Description:** Certificate number (if applicable). 

#### ▸ Field: `Type of Assessment`
- **Data Type:** `Reference List (Integer)`
- **Code:** `TOA`
- **Input from User:** Yes
- **Mandatory:** Yes
- **Description:** Dropdown list – user must select one of the predefined assessment types from a linked set of values.
- **Note:** This field is a single-choice reference linked to a value set

#### ▸ Field: `Manufacturer(s) name and SRN`
- **Data Type:** `String`
- **Code:** `MANSRN`
- **Input from User:** Yes
- **Mandatory:** Yes
- **Description:** Full legal name of the manufacturer(s) and their Single Registration Number (SRN), as registered in EUDAMED.

#### ▸ Field: `Authorized representative (if applicable) name and SRN`
- **Data Type:** `String`
- **Code:** `ARSRN`
- **Input from User:** Yes
- **Mandatory:** No
- **Description:** Name and Single Registration Number (SRN) of the authorized representative, if applicable. Required only for manufacturers based outside the EU.

#### ▸ Field: `Job (Contract) Number`
- **Data Type:** `String`
- **Code:** `JCN`
- **Input from User:** Yes
- **Mandatory:** Yes
- **Description:** Internal job or contract number associated with the assessment. Required for project tracking and documentation.

#### ▸ Field: `Rules Annex`
- **Data Type:** `Reference List (Integer)`
- **Code:** `ANN7`
- **Input from User:** Yes
- **Mandatory:** Yes
- **Description:** Selection of the applicable rule from the IVDR Annex VIII.  
- **Note:** This dropdown is linked to the next field, **Risk Class**, and determines its value based on the selected rule.

#### ▸ Field: `Risk Class`
- **Data Type:** `Reference List (Integer)`
- **Code:** `RCLASS`
- **Input from User:** Yes *(partially auto-filled)*
- **Mandatory:** Yes
- **Description:** The risk class (A, B, C, or D `→` D is out of scope for IVD team) of the in-vitro diagnostic device, according to IVDR classification rules.  
- **Note:** This field is linked to `RULEANNEX`. It is often auto-filled based on the selected rule, but in some cases, users may need to choose manually between multiple applicable classes.

#### ▸ Field: `Technical Documentation`
- **Data Type:** `String`
- **Code:** `TD`
- **Input from User:** Yes
- **Mandatory:** Yes
- **Description:** Identifying name or number and version of the Technical Documentation submitted for assessment.

#### ▸ Field: `Starting Date and Time of TDA Report`
- **Data Type:** `Date & Time`
- **Code:** `TDASTART`
- **Input from User:** No *(Automatically generated)*
- **Mandatory:** Yes
- **Description:** Timestamp of when the TDA Report is created. This field is system-generated and captures both date and time for audit purposes.
- **Note:** Value is calculated automatically at the time of report creation.
- **Expression:** `UTCnow()`

#### ▸ Field: `Ending Date and Time of TDA Report`
- **Data Type:** `Date & Time`
- **Code:** `DECDATA`
- **Input from User:** No *(Automatically calculated)*
- **Mandatory:** No
- **Description:** Timestamp capturing the final declaration of the TDA Report by the Product Assessor. Represents the formal completion date and time.
- **Note:** This field is populated **only** when the confirmation checkbox (`PCONF`) is ticked.
- **Expression:** `if([PCONF] = 1, utcnow(), NULL)`

#### ▸ Field: `Due Date`
- **Data Type:** `Date`
- **Code:** `DUEDATE`
- **Input from User:** Yes *(if applicable)*
- **Mandatory:** No
- **Description:** The deadline or due date for completing the TDA Report, if applicable.  
- **Note:** This field is optional and only filled in when a due date is specified.

#### ▸ Field: `Intended Purpose of the Device`
- **Data Type:** `Multiline String`
- **Code:** `PURP`
- **Input from User:** Yes
- **Mandatory:** Yes
- **Description:** The intended medical purpose of the in-vitro diagnostic device, as stated by the manufacturer. This is a mandatory free-text field and must be clearly defined.

#### ▸ Field: `Product Assessor(s) Name`
- **Data Type:** `Embedded Table`
- **Code:** `PALIS`
- **Input from User:** Yes *(Selection from predefined list)*
- **Mandatory:** Yes
- **Description:** Selection of one or more Product Assessors involved in preparing the TDA Report.
- **Embedded Table:** `PA List` (Subordinate table description: `PAs`, code: `PAS`)
- **Selection Source:** Linked set of persons with the role `"Competence record Owner"` from the Notified Body.
- **Link Type:** `Person`
- **Structure:** Variable number of lines (one per selected Product Assessor).
- **Note:** The user selects Product Assessor(s) from a controlled list derived from internal organizational roles. Multiple assessors can be added to the same report.
#### 🔐 Permissions Notes:
The default role `Competence record Owner` does **not** allow members to view or edit each other’s inspections.  
To support collaborative report writing, a **new custom role** is maybe recommended.

#### ▸ Field: `Final Reviewer Name`
- **Data Type:** `Label`
- **Code:** `FLABEL`
- **Input from User:** No *(Read-only fixed value)*
- **Mandatory:** Yes
- **Description:** Name of the Final Reviewer assigned to this report using the Label For Free use
Currently set as a fixed field — logic for assignment is under review.

#### ▸ Field: `Clinical Reviewer Name`
- **Data Type:** `Label
- **Code:** `FLABEL`
- **Input from User:** No *(Read-only fixed value)*
- **Mandatory:** Yes
- **Description:** Name of the Clinical Reviewer involved in the assessment using the Label For Free use  
Currently set as a fixed field — selection logic is still to be defined.

#### ▸ Field: `Current Review Status`
- **Data Type:** `Reference List (Integer)`
- **Code:** `CURRS`
- **Input from User:** Yes
- **Mandatory:** Yes
- **Description:** Indicates the current state of the TDA Report review process.
- **Set of Values:**  
  - `Ongoing, further response required`   
  - `Completed, certificate reccomended`
  - `Rejected and stopped` 
- **Note:** Selection is done from a controlled **dropdown list** to ensure consistency in reporting lifecycle status.

#### ▸ Field: `Applicable Codes`
- **Data Type:** `Embedded Table`
- **Code:** `APPLICABLE`
- **Input from User:** Yes *(Selection from predefined set)*
- **Mandatory:** Yes
- **Description:** A dynamic list of applicable codes related to the TDA Report.  
  The user selects one or more codes from a predefined set of values.
- **Embedded Table:** `Applicable Codes`
- **Selection Source:** Prefilled set of applicable codes derived from a set of values.
- **Structure:** Variable number of lines (each row represents one selected code) - Subordinate table (child table).
- Set of value **Modifiable via:** `Reference Tab. Applicable Codes` *(Reference table code: `APPLICABLE`)*
- **Note:** The user can select multiple codes as needed. The set of available codes is pre-defined, ensuring consistency in reporting.
### 3. **Subordinate Table: `Requirements`** (Embedded within `TDA Big Table Requirements`)

#### ▸ **Field: `TDA Big Table Requirements`**
- **Data Type:** `Embedded Table`
- **Code:** `TDABIG`
- **Input from User:** Yes _(Content to be entered inside each requirement)
- **Mandatory:** Yes
- **Description:** This field embeds the full structure of the TDA assessment sections. It contains a hierarchical table of predefined requirements that must be filled by the Product Assessor(s).
- **Embedded Table Name:** `Requirements`
- **Embedded Table Code:** `REQ`
- **Structure Type:** Subordinate table (child table)
- **Prefilled Set:** `Big Table of Sections ALL`
- **Line Type:** `Fixed Lines`
- **Line Content:** Each line corresponds to a specific requirement (or paragraph) within a section of the TDA Report.
- **User Interaction:** The user is expected to enter relevant content, and/or assessment outcomes for each requirement.
- **Purpose:** This is the central working table for the Product Assessor to document compliance against IVDR requirements in a structured, section-by-section format.

##### ▸ **Field: `Paragraph`**

- **Data Type:** `Reference List (Integer)`
- **Code:** `PNUM`
- **Input from User:** No _(Automatically prefilled via Set of Values)_
- **Mandatory:** Yes
- **Description:** Identifies the specific paragraph (requirement section) that each row in the `Requirements` table refers to.  
    Each paragraph corresponds to a predefined assessment item within the TDA structure under IVDR.
- **Reference Table:** `Requirements Tab. Requirements (Code: REQ_REF, key: REQ_ID == Paragraph)
- **Prefilled Set:** Automatically generated from the values in the reference table.
- **Role in Structure:** This field acts as a key identifier and determines the content of each fixed line in the subordinate table.
- **Dynamic Behavior:**  
    The use of a reference table allows full flexibility: when new paragraphs are added to the reference, the list of requirements can be extended accordingly.
    ⚠️ However, when updating the list of paragraphs, it is **crucial to also update the Prefilled Set** in the configuration of the subordinate table `REQ`, in order for the new paragraphs to be included properly.



---
### **Main Table – Field Summary**
Page 1: TDA Header 

| **Field Name**                                    | **Data Type**            | **Code**     | **User Input**       | **Mandatory** | **Notes / Description**                                                                                   |
| ------------------------------------------------- | ------------------------ | ------------ | -------------------- | ------------- | --------------------------------------------------------------------------------------------------------- |
| Label                                             | None                     | `NBINF`      | No                   | No            | Fixed label: Notified Body: SGS Belgium NV – Notified Body No: 1639                                       |
| Panel: Admin. Particulars                         | Panel                    | `PAN`        | No                   | No            | Groups multiple related fields (manufacturer, product, etc.)                                              |
| └ In-vitro diagnostic device name, model and type | String                   | `IVDNMT`     | Yes                  | Yes           | Full name of the IVD device                                                                               |
| └ Basic UDI-DI(s)                                 | String                   | `UDI-DI`     | Yes                  | No            | If available – unique identifier from the manufacturer                                                    |
| └ Certificate Number                              | String                   | `CERTN`      | Yes                  | No            | If applicable                                                                                             |
| └ Type of Assessment                              | Reference List (Integer) | `TOA`        | Yes (Dropdown)       | Yes           | Linked to predefined set of values                                                                        |
| └ Manufacturer(s) name and SRN                    | String                   | `MANSRN`     | Yes                  | Yes           | Legal name & SRN of manufacturer                                                                          |
| └ Authorized Representative name and SRN          | String                   | `ARSRN`      | Yes                  | No            | Required if the manufacturer is outside the EU                                                            |
| └ Job (Contract) Number                           | String                   | `JCN`        | Yes                  | Yes           | Internal reference used for tracking                                                                      |
| └ Rules Annex                                     | Reference List (Integer) | `ANN7`       | Yes (Dropdown)       | Yes           | Linked to IVDR Annex VIII – affects the value of the **Risk Class** field                                 |
| └ Risk Class                                      | Reference List (Integer) | `RCLASS`     | Yes (Partially auto) | Yes           | A/B/C class according to IVDR, often auto-filled based on **Rules Annex**                                 |
| └ Technical Documentation                         | String                   | `TD`         | Yes                  | Yes           | Identifying doc name + version                                                                            |
| └ Starting Date and Time of TDA Report            | Date & Time              | `TDASTART`   | No (Auto-generated)  | Yes           | Automatically filled at report creation: `UTCnow()`                                                       |
| └ Ending Date and Time of TDA Report              | Date & Time              | `DECDATA`    | No (Calculated)      | No            | `if([PCONF] = 1, utcnow(), NULL)` – Filled when the Product Assessor confirms report                      |
| └ Due Date                                        | Date                     | `DUEDATE`    | Yes                  | No            | Optional field for specifying a deadline                                                                  |
| └ Intended Purpose of the Device                  | Multiline String         | `PURP`       | Yes                  | Yes           | Free-text description from manufacturer                                                                   |
| └ Product Assessor(s) Name                        | Embedded Table           | `PALIS`      | Yes (Selection)      | Yes           | Select from persons with role “Competence Record Owner” in the organization. Supports multiple lines      |
| └ Final Reviewer Name                             | Label                    | `FLABEL`     | No (Fixed)           | Yes           | Placeholder – logic for reviewer assignment under review                                                  |
| └ Clinical Reviewer Name                          | Label                    | `FLABEL`     | No (Fixed)           | Yes           | Placeholder – logic for clinical reviewer to be defined                                                   |
| └ Current Review Status                           | Reference List (Integer) | `CURRS`      | Yes (Dropdown)       | Yes           | Values: _Ongoing, Completed, Rejected_                                                                    |
| └ Applicable Codes                                | Embedded Table           | `APPLICABLE` | Yes (Selection)      | Yes           | Embedded table with variable lines; editable from `Reference Tab. Applicable Codes`                       |
| [[#📑 Subordinate Table `Requirements`]]          | Embedded Table           | `TDABIG`     | No (Fixed)           | Yes           | Main structure for documenting compliance against IVDR requirements, embeds a prefilled table of sections |

### 📑 Subordinate Table: `Requirements`
(Embedded within `TDA Big Table Requirements` – Code: `TDABIG`)
Page 2 and Sections: TDA Overview

/*table to be done*/



---
### *More Subordinate tables to be added...*
