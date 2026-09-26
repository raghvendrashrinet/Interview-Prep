## One Page Lookup
### Governance through 
    - Hieracrhy -- Management Groups
         - MGMT Groups( ensure reasonably Flat hierarchy(3 to 4) don't complicate)
         - subscription
         - RG
         _ resources

           - Policy (use compliance Dashboard to analyze overall state of env)
           - RBAC (Permission-> user/gp/sa-> scope) rbac are additive
           - Budget
           - tags(use Policy to enforce tagging) 

        ### Locks ( Subscription/RG/Resource
           - Read only
           - Can't Delete 
           ***Most restrictive Lock has precedence***
  #### Landing Zones : To ensure key foundational priniciples are put in place 
       landing zones are MGMT groups and subscriptions - Platform landing zones, Application landing zones - LZ are provisioned through IaC

 ### IAM : 3 Choices
   - `MS Entra ID` : multitenant cloud based identity management service
   - `MS Entra External ID (B2B)`: business parterners invited , authetication done at there idenety
   - `MS Entra External ID (B2C)`: lets communicate your App and external SAML/OIDC providers,Register APP in B2C,define SAMLE provider and we customer accesses app it asked to choose its saml provider to autheticate,ie customers use there identity (Bring your own Id- BYOI), Azure b2c redirect user to gioogle(SAML?OIDC provider) to authenticatie ,token back to B2C

### Entra ID Core Services Differentiation
- `Conditional Access `(The Zero Trust Decision Engine): Evaluates real-time signals (user, device, location, app, risk level) to grant, block, or enforce MFA.
- `Entra ID Protection `(Risk Detection): Computes user risk and sign-in risk using telemetry and machine learning models. Key insight: Risk detections feed directly into Conditional Access as policy signals
- `Privileged Identity Management / PIM (Just-In-Time Elevation)`: Enforces time-bound, approval-based role activation, justification, and access review workflows for high-privilege roles (e.g., Global Admin, Contributor).
- Access Review : Who accessed what
**PIM & Access Review are P2 Feature**

```
[ User Sign-in ] ──> [ Entra ID Protection ] ──(Risk Signal)──> [ Conditional Access ] ──> [ Granted / MFA / Blocked ]
                                                                        │
                                                            [ Just-In-Time Elevation ]
                                                                        │
                                                             [ PIM Role Activation ]
```  
