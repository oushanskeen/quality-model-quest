
> [!NOTE] Document Legend
>
This document contains three types of content:
>
>- #idea - Defines intent, philosophy, and mental models
>- #rules - Defines mandatory rules and guarantees
>- #implementation - Describes how the framework is applied in practice
>
Readers interested in *using* the framework should focus on #procedural  sections.

## Contents

[[#PART I — CONCEPTS]]
[[#1. Abstract TL;DR concept]]
[[#2. Motivation Why it must be built? concept]]
[[#3. Spec What must be built? normative]]
[[#4. Rationale Why this approach? concept]]
[[#5. Backwards Compatibility How does it change ? normative]]

[[#PART II — APPLYING THE FRAMEWORK]]
[[#6. Implementation How it was built ? procedural]]
[[#7. Risks/Dependencies What can go wrong? concept]]
[[#8. Scope In/Out What's in/out? normative]]
[[#9. Metrics How the success is measured ? normative]]

---

# PART I — CONCEPTS 
_This part defines the **contract, preserved qualities, and governance rules**.
It specifies **what must always be true**, independent of implementation._

---
# 1. Abstract: TL;DR #idea 
ISO/IEC 25010 inspired Quality Model Diagram (QMD),  helps to explicitly layer expectations into dimensions, break dimensions down into metrics, tie them to normalized signals, and assign viability constraints to metrics.
_for brief  overview watch QualityModelDiagram.pdf_

---
# 2. Motivation: Why it must be built? #idea 

Without QMD (i.e. without quality model) only behavior agreements (how systems should act) as tests and change agreements (implementation plan) as tickets are available. No viability constraints (operational bounds defining system health) and  no impact agreements (how changes affect quality). No system-level quality view as a result.
With QMD, viability is explicitly defined and agreed, and impact is modeled and traceable.

---
# 3. Spec: What must be built? #rules 

Visualization includes a radial tree for structure (dimensions & metrics)
and circular coordinates for system signals. The key idea is to define
Quality as low variability within viability constraints, i.e. the system
MUST behave consistently, stay within defined limits,and signal where to
focus and whether changes are effective.

## Glossary

1. **Quality**: An explicitly defined, observable property of a system
2. **Viability constraint**: operational bounds defining system health
3. **Impact agreement**: change-to-quality affect description 

---
## Specification Requirements

### Required
**SR-RQ01**: Hierarchy
**SR-RQ02**: [Quality Model Questionnaire](./Quality%20Model%20Questionnaire.md) 
**SR-RQ03**: Dimensions
**SR-RQ04**: Normalized Signals
**SR-RQ05**: Viability Bounds
**SR-RQ06**: Signals API
### Recommended
**SR-RC0?**: #TBA❓ 
### Optional
**SR-OP0?**: #TBA❓  

---
## 4. Rationale: Why this approach? #rules  

**With QMD**, viability is explicitly defined and agreed, and impact [9] is modeled and traceable.
**Without QMD** (i.e. without quality model) only behavior agreements (how systems should act) as tests and change agreements (implementation plan) as tickets are available. No viability constraints (operational bounds defining system health) and  no impact agreements (how changes affect quality). No system-level quality view as a result.

**Quality Model Rationale**

| **Dimension**         | **Quality Model**                                                                             | **Ad-hoc Testing**                                  | **Automation-Only Strategy**                                   | **SRE / Error Budget Model**                                 | **Dev Owns Quality**                                           | **QA Owns Quality**                                         | **Product Metrics Only**                                           |
| --------------------- | --------------------------------------------------------------------------------------------- | --------------------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------ |
| Change Effect Measure | ++++++++++ 10/10<br>Shows clearly if change helped or hurt                                    | ++--------<br>2/10<br>Hard to link cause and effect | +++++-----<br>5/10 <br>Shows tests, not real impact            | +++++++++-<br>9/10<br>Clear for reliability                  | ++++++----<br>6/10<br>Depends on tracking                      | +++++-----<br>5/10 <br>Partial visibility                   | +++++++++-<br>9/10<br>Clear user impact, not cause                 |
| Cost                  | +++++++++-<br>1/10<br>Saves money                                                             | ++--------<br>8/10 <br>Cheap first, expensive later | ++++------<br>4/10<br>Can waste effort                         | ++--------<br>2/10<br>Saves downtime cost                    | ++++------<br>4/10<br>Varies                                   | +++++-----<br>5/10 <br>Slows later                          | ++++++++++<br>10/10<br>Late fixes cost                             |
| Stakeholder Alignment | ++++++++++<br>10/10<br>Everyone shares same view of quality                                   | ++--------<br>2/10 Everyone sees things differently | +++++-----<br>5/10<br>Dev/test aligned, others not             | +++++++---<br>7/10<br>Devs aligned on uptime                 | ++++++----<br>6/10 <br>Team-level alignment only               | ++++------<br>4/10<br>QA vs dev vs product split            | ++++++----<br>6/10 <br>Product aligned, devs not fully             |
| Coverage              | ++++++++++<br>10/10<br>Checks everything                                                      | +++-------<br>3/10<br>Only basics                   | +++++-----<br>5/10<br>Misses big picture                       | ++++++----<br>6/10<br>Mostly uptime                          | ++++++----<br>6/10<br>Depends on team                          | ++++++----<br>6/10<br>Limited by QA scope                   | ++++------<br>4/10<br>Only user view                               |
| Measurability         | ++++++++++<br>10/10<br>Clear numbers                                                          | ++--------<br>2/10<br>Guessing                      | ++++++----<br>6/10<br>Some metrics                             | ++++++++--<br>8/10<br>Strong uptime metrics                  | ++++------<br>4/10<br>Not consistent                           | +++++-----<br>5/10<br>Limited QA metrics                    | ++++++++--<br>8/10<br>Strong business metrics                      |
| Scalability           | ++++++++++<br>10/10 Scales well                                                               | ++--------<br>2/10<br>Breaks fast                   | ++++++----<br>6/10<br>Needs effort                             | ++++++++--<br>8/10<br>Scales in prod                         | ++++++----<br>6/10<br>Depends on discipline                    | ++++------<br>4/10<br>QA bottleneck                         | +++++++---<br>7/10<br>Metrics scale                                |
| Decision Support      | ++++++++++<br>10/10<br>Clear choices                                                          | ++--------<br>2/10 <br>Guesswork                    | ++++------<br>4/10<br>Weak signals                             | ++++++++--<br>8/10<br>Good for uptime                        | +++++-----<br>5/10<br>Local only                               | +++++-----<br>5/10<br>Partial view                          | +++++++---<br>7/10<br>Business view only                           |
| Build Risk            | ++++++----<br>6/10 Medium setup complexity                                                    | +---------<br>1/10 <br>No setup risk                | +++++-----<br>5/10<br>Risk of overbuilding                     | +++++----<br>6/10<br>Risk of wrong SLOs                      | +++++++---<br>7/10<br>Risk of inconsistency                    | +++++++---<br>7/10<br>QA bottleneck risk                    | ++++++----<br>6/10<br>Wrong metrics risk                           |
| Usage Risk            | +++++-----<br>5/10<br>Risk of over-measuring                                                  | ++++++++++<br>10/10<br>High failure risk            | +++++++---<br>7/10<br>Blind spots likely                       | +++++++---<br>7/10 <br>Misses non-reliability issues         | ++++++++--<br>8/10 <br>Uneven quality                          | ++++++++--<br>8/10<br>Gaps in ownership                     | +++++++++-<br>9/10 <br>Late discovery of issues                    |
| Summary               | **Gains**: full view, clear decisions, aligned teams<br>**Losses**: setup effort (not really) | **Gains**: simple start<br>**Losses:** chaos later  | **Gains**: repeatable tests<br>**Losses:** missing system view | **Gains**: stable systems<br>**Losses:** missing system view | **Gains:** strong ownership<br>**Losses:** missing system view | **Gains:** clear QA role<br>**Losses:** missing system view | **Gains:** strong business view<br>**Losses:** missing system view |

**Quality Model Diagram Rationale**
#TBA❓ 

---
## 5. Backwards Compatibility: How does it change ? #rules
The framework is governed by a **fixed ontology of nine Index parts**, as defined in the [Tool Ontology Graph](Tool%20Ontology%20Graph.md) (and used here) and fully describe in [Change Governance Model](Change%20Governance%20Model.md). These parts represent **conceptual levels**, not just document sections. Every change **must be expressed against one or more parts**, ensuring intent, guarantees, and enforcement remain aligned.
### Nine Governance Levels
1. **Abstract** - summary contract of intent
2. **Motivation** - problem framing and justification
3. **Spec** - normative requirements
4. **Rationale** - comparative reasoning
5. **Backwards Compatibility** - change constraints
6. **Implementation** - concrete realization and lifecycles
7. **Risks** - failure modes and mitigations
8. **Scope** - boundaries and exclusions
9. **Metrics** - validation and success signals
    
Changes propagate along **ontology edges**, so updating one part may require reviewing dependent parts (e.g., changing the Implementation triggers checks in Spec, Scope, Metrics, and Risks). Changes are classified as **Form-, Rule-, or Model-level** depending on their impact.
This approach prevents drift, enforces dependency awareness, and makes quality **observable, enforceable, and maintainable**.

> For full governance rules, examples, and classification guidance, see the [Change Governance Model](Change%20Governance%20Model.md)

#TBA❓ Add a point about DECISION MEMORY

# PART II — APPLYING THE TOOL
_This part describes **how the tool is applied in practice**
using a reference implementation and lifecycle description._

---
## 6. Implementation: How it was built ? #form
_(Current reference implementation; structure may evolve)_
[Temp demo link](https://observablehq.com/d/14bffbd42c4a4d6e)
### Lifecycles
#TBA❓ 

---
## 7. Risks/Dependencies: What can go wrong? #rules 
#TBA❓

---
## 8. Scope In/Out: What's in/out? #implementation  
#TBA❓ 

---
#### In:
#TBA❓ 
#### Out:
#TBA❓ 
## 9. Metrics: How the success is measured ? #implementation  

#TBA❓

> **Notes:**
> - **SDLC Task IDs** encode stage: RS=Requirements, DS=Design, DV=Development, DP=Deployment, VS=Validation
> - **AC** = Acceptance Criteria
> - **SC** = Success Criteria