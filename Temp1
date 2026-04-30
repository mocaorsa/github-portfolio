## Zero Trust Architecture Diagram – Golden State Water Treatment Facility

```mermaid
flowchart TB

%% =========================
%% USERS
%% =========================
User1[HR Admin]
User2[HR Analyst]
User3[OT Operator]
User4[OT Engineer]

%% =========================
%% IDENTITY & POLICY
%% =========================
IdP[Identity Provider (SSO + MFA)]
PE[Policy Engine]
PDP[Policy Decision Point]
PIP[Policy Information Point\n(Device Health, Risk, Location, Behavior)]
SIEM[SIEM / UEBA]
PAM[Privileged Access Management (JIT)]

%% =========================
%% ENFORCEMENT
%% =========================
IAP[Identity-Aware Proxy / ZTNA Gateway]
PEP[Policy Enforcement Point]
VPN[Managed VPN (Corp Devices Only)]

%% =========================
%% IT RESOURCES (HR)
%% =========================
subgraph IT Zone (Confidentiality Focus)
HR1[HR Files Server]
HR2[Payroll PII Repository]
HR3[Certification Records System]
end

%% =========================
%% OT RESOURCES
%% =========================
subgraph OT Zone (Safety & Availability Focus)
SCADA[SCADA Control System]
PLC[PLC Controllers]
CHEM[Chemical Dosing System]
HIST[Historian Database]
PUMP[Remote Pump Stations]
end

%% =========================
%% USER AUTH FLOW
%% =========================
User1 --> IdP
User2 --> IdP
User3 --> IdP
User4 --> IdP

IdP --> PE
PIP --> PE
SIEM --> PIP
PE --> PDP
PDP --> IAP
PDP --> PAM

%% =========================
%% IT ACCESS FLOW
%% =========================
IAP --> HR1
IAP --> HR2
IAP --> HR3

%% =========================
%% OT ACCESS FLOW
%% =========================
User3 --> VPN
User4 --> VPN
VPN --> IAP
IAP --> SCADA
SCADA --> PLC
SCADA --> CHEM
SCADA --> HIST
SCADA --> PUMP

%% =========================
%% PRIVILEGED ACCESS FLOW
%% =========================
User4 --> PAM
PAM --> IAP

%% =========================
%% MONITORING LOOP
%% =========================
HR1 --> SIEM
SCADA --> SIEM
PLC --> SIEM
HIST --> SIEM
```
