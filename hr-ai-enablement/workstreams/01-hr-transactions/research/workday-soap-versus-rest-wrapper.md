# Workday ESS sample: direct SOAP versus the existing REST wrapper

**Research date:** 23 September 2026. **New user-confirmed information:** the existing SOAP-to-REST wrapper supports user, ISU, and OAuth authentication. Implementation and permission behavior have not been inspected.

## What the Microsoft sample actually does

The vacation-balance XML selects `Absence_Management`, version `v42.0`, declares `<authType>User</authType>`, and contains `Get_Time_Off_Plan_Balances_Request`. It provides employee and effective-date parameters, then uses XPath expressions to extract balance, plan, and unit fields. This is a SOAP service request template; the file does not itself contain the complete transport or token-handling implementation. [Sample XML](https://github.com/microsoft/CopilotStudioSamples/blob/main/EmployeeSelfServiceAgent/Workday/EmployeeScenarios/EmployeeGetVacationBalance/msdyn_HRWorkdayHCMEmployeeGetVacationBalance.xml)

The topic takes the employee identifier from `Global.ESS_UserContext_Employee_Id`, supplies the date, and invokes `WorkdaySystemGetCommonExecution`. The sample therefore depends on the installed ESS Workday execution machinery. It does not show a model independently constructing and sending arbitrary SOAP requests. It is also a read scenario, not evidence of write approval or retry safety. [Topic YAML](https://github.com/microsoft/CopilotStudioSamples/blob/main/EmployeeSelfServiceAgent/Workday/EmployeeScenarios/EmployeeGetVacationBalance/topic.yaml)

Microsoft's extensibility guide separately describes a SOAP time-off submission scenario using request/response templates and a topic. It labels extensibility a customization beyond the documented baseline and requires appropriate Workday business-process permissions. [Extensibility guide](https://learn.microsoft.com/en-us/microsoft-365/copilot/employee-self-service/workday-extensibility)

## The actual choice

Both proposed paths may ultimately use the same Workday SOAP operation:

- Microsoft ESS topic → common execution / Workday connector → Workday SOAP.
- Claude tool or Copilot action → company business API → existing wrapper → Workday SOAP.

The decision is where to own authentication, mappings, validation, and operational controls. SOAP is not inherently incompatible with OAuth, and REST is not inherently safer or more reliable. Microsoft's setup documents a SOAP connector with Entra-integrated authentication and Workday OAuth configuration. [Integration setup](https://learn.microsoft.com/en-us/microsoft-365/copilot/employee-self-service/workday)

| Consideration | Microsoft ESS SOAP path | Existing REST wrapper path |
|---|---|---|
| Extending an already configured ESS installation | Closely follows Microsoft's sample architecture | Requires a custom action/connector and topic integration |
| Sharing business operations across Claude, Copilot, and other applications | ESS templates/execution cannot be assumed portable | A reusable company API can support multiple adapters |
| SOAP schema handling | Maintained in ESS template configuration | Maintained centrally in wrapper/service mappings |
| Infrastructure ownership | Uses Microsoft package and connector dependencies | Adds company runtime, support, and release responsibility |
| Identity | Depends on selected connection and Workday permissions | User reports user/ISU/OAuth support; validate exact identity behavior |
| Transaction controls | Inspect each configured execution path | Can be centralized, but are not guaranteed by format conversion |

## Recommendation for this company

Favor the existing wrapper for shared HR operations intended to work across Claude Desktop, Copilot Studio, and future clients. Its reported authentication support makes it more valuable than a basic XML-to-JSON adapter. Add narrow, versioned business contracts and required policy/transaction controls where missing rather than rebuilding functionality already present.

Retain the Microsoft ESS SOAP path as a valid option for scenarios that fit the installed accelerator well. Do not rewrite working ESS scenarios solely to remove XML. A mixed architecture is reasonable if identity, approvals, audit, and support ownership remain clear on both paths.

Replacing the sample's endpoint with a REST URL is not established as a supported substitution. The shown template contains Workday SOAP-specific service, version, request, and XPath mappings. Use a custom REST connector/action or an MCP tool for the wrapper and adapt the topic's invocation. Verify any alternative extension hook in the installed package before relying on it.

## Authentication: distinguish identity from protocol

“User” and “ISU” describe whose authority is used. OAuth describes an authorization mechanism and can authorize access associated with either kind of identity. An OAuth-authenticated ISU is still a service identity, not automatically the HR operator's delegated identity.

Validate two separate boundaries: client → company service, and company service → Workday. Confirm whether the wrapper's OAuth support covers one or both, how refresh/revocation works, and whether source credentials are linked to the correct operator. Never forward a token to an API for which it was not issued.

For the employee self-service balance tool, derive the employee identifier from authenticated context. For an HR-administrator tool, allow a target worker only after record/population authorization. Keep authority selection in server configuration; the model must not choose a more privileged ISU to retry a denied user request.

## First comparison to run

Implement the same balance lookup through the existing ESS path and the wrapper in a test tenant, with the same actor and effective date. Compare Workday results, units, multi-plan/multi-position associations, error handling, audit identity, and revocation. Exercise an unauthorized worker identifier. Then repeat with one bounded write, adding exact-change approval, native business-process preservation, duplicate protection, and timeout reconciliation.

The sample's flattened XPath outputs make plan/balance association a specific item to validate; inspection alone does not establish how the shared runtime aligns repeated values. A wrapper API can expose explicit structured records, but its mapping still needs correctness tests.

This note is based on direct inspection of the sample README, XML, YAML, and Microsoft documentation. It is not a live integration test or a claim of full package behavior.
