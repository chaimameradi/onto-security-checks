# Onto-Security Checks

**An Ontology-Based Reasoning Approach for Automated Security 
Verification in Software Architectures**

*Meradi Chaima, Aouag Sofiane, Beloucif Assia*  
*University of Batna 2, Algeria*

---

## Description

This repository contains the RDF/OWL security ontologies developed 
for the Onto-Security Checks approach, applied to two case studies:

- **DVWA** (Damn Vulnerable Web Application)
- **PMS** (Pharmacy Management System)

---

## Files

| File | Description |
|------|-------------|
| `DVWASECURITY.rdf` | Security ontology for DVWA |
| `PMSontology.rdf`  | Security ontology for PMS  |

---

## Ontology Details

### Classes

| Class | Description |
|-------|-------------|
| User | Entity that interacts with the system |
| System | Technical components (servers, apps, DBs) |
| Vulnerability | Weakness exploitable by a threat |
| Threat | Potential attack vector |
| Countermeasure | Defensive mechanism |
| Risk | Potential loss when threat exploits vulnerability |
| AtRiskSystem | System with at least one unmitigated risk |
| MitigatedSystem | System with deployed countermeasures |
| MitigatedRisk | Risk addressed by a countermeasure |

### Object Properties

| Property | Domain → Range | Description |
|----------|---------------|-------------|
| hasAccessTo | User → System | User access privileges |
| exploits | Threat → Vulnerability | Attack vector linkage |
| impacts | Threat → System | Direct/indirect impact |
| protectedBy | System → Countermeasure | Defensive association |
| mitigates | Countermeasure → Threat | Threat neutralization |
| reduces | Countermeasure → Risk | Risk reduction |

### SWRL Rules

| Rule | Description |
|------|-------------|
| R1 | Risk inference from threat-vulnerability chain |
| R2 | Risk type materialization |
| R3 | Global mitigation via effective countermeasure |
| R4 | Targeted vulnerability mitigation |
| R5 | AtRiskSystem labeling |

---

## SPARQL Queries

Four SPARQL queries were used for analysis:

| Query | Target |
|-------|--------|
| Q1 | All risks of the system |
| Q2 | Critical unmitigate
