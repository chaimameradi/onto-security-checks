# Onto-Security Checks

**An Ontology-Based Reasoning Approach for Automated Security Verification in Software Architectures**

*Meradi Chaima, Aouag Sofiane, Beloucif Assia*
*University of Batna 2, Algeria*

---

## Description

This repository contains the RDF/OWL security ontologies developed for the Onto-Security Checks approach, applied to two case studies:

- **DVWA** (Damn Vulnerable Web Application)
- **PMS** (Pharmacy Management System)

---

## Files

| File | Description |
|------|-------------|
| `DVWASECURITY.rdf` | Security ontology for DVWA |
| `PMSontology.rdf` | Security ontology for PMS |

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
|----------|----------------|-------------|
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

### DVWA

**Q1 — All risks of DVWA_WebApp**
```sparql
PREFIX dwa: <http://www.semanticweb.org/quicktech/ontologies/2025/10/dvwaontologie#>
SELECT ?risk WHERE {
  dwa:DVWA_WebApp dwa:hasRisk ?risk
} ORDER BY ?risk
```

**Q2 — Critical unmitigated risks**
```sparql
PREFIX dwa: <http://www.semanticweb.org/quicktech/ontologies/2025/10/dvwaontologie#>
SELECT ?risk WHERE {
  dwa:DVWA_WebApp dwa:hasRisk ?risk .
  FILTER NOT EXISTS { ?risk a dwa:MitigatedRisk }
} ORDER BY ?risk
```

**Q3 — AtRisk systems without countermeasures**
```sparql
PREFIX dwa: <http://www.semanticweb.org/quicktech/ontologies/2025/10/dvwaontologie#>
SELECT ?system WHERE {
  ?system a dwa:AtRiskSystem .
  FILTER NOT EXISTS { ?system dwa:protectedBy ?countermeasure }
} ORDER BY ?system
```

**Q4 — Countermeasure coverage**
```sparql
PREFIX dwa: <http://www.semanticweb.org/quicktech/ontologies/2025/10/dvwaontologie#>
SELECT ?system (COUNT(?countermeasure) AS ?count) WHERE {
  ?system dwa:protectedBy ?countermeasure
} GROUP BY ?system ORDER BY ?system
```

### PMS

**Q1 — All risks of PharmacyWebApp**
```sparql
PREFIX pms: <http://www.semanticweb.org/quicktech/ontologies/2025/10/pmsontoologie#>
SELECT ?risk WHERE {
  pms:PharmacyWebApp pms:hasRisk ?risk
} ORDER BY ?risk
```

**Q2 — Critical unmitigated risks**
```sparql
PREFIX pms: <http://www.semanticweb.org/quicktech/ontologies/2025/10/pmsontoologie#>
SELECT ?risk WHERE {
  pms:PharmacyWebApp pms:hasRisk ?risk .
  FILTER NOT EXISTS { ?risk a pms:MitigatedRisk }
} ORDER BY ?risk
```

**Q3 — AtRisk systems without countermeasures**
```sparql
PREFIX pms: <http://www.semanticweb.org/quicktech/ontologies/2025/10/pmsontoologie#>
SELECT ?system WHERE {
  ?system a pms:AtRiskSystem .
  FILTER NOT EXISTS { ?system pms:protectedBy ?countermeasure }
} ORDER BY ?system
```

**Q4 — Countermeasure coverage**
```sparql
PREFIX pms: <http://www.semanticweb.org/quicktech/ontologies/2025/10/pmsontoologie#>
SELECT ?system (COUNT(?countermeasure) AS ?count) WHERE {
  ?system pms:protectedBy ?countermeasure
} GROUP BY ?system ORDER BY ?system
```

---

## How to Open

1. Download `DVWASECURITY.rdf` or `PMSontology.rdf`
2. Open with **Protégé 5.6.7**
   → File → Open → select the `.rdf` file
3. Enable **HermiT reasoner**
   → Reasoner → HermiT → Start reasoner
4. Run SWRL rules
   → Windows → Tabs → SWRLTab → Run Drools
5. Execute SPARQL queries for analysis

---

## Results

### DVWA

| Query | Target | Results | Time (s) |
|-------|--------|---------|----------|
| Q1 | All risks | Database_Compromise, Session_Hijacking, System_Takeover | 0.43 |
| Q2 | Critical unmitigated risks | None (all risks mitigated) | 0.29 |
| Q3 | AtRisk without countermeasures | Apache_Server, DVWA_SQLModule, MySQL_DB | 0.49 |
| Q4 | Countermeasure coverage | 6 on DVWA_WebApp, 1 on Apache_Server | 0.35 |

| Scenario | Detection Rate | Precision |
|----------|---------------|-----------|
| SQL Injection | 1.00 | 1.00 |
| XSS | 1.00 | 1.00 |
| Command Injection | 1.00 | 1.00 |
| Session Mgmt. | 1.00 | 1.00 |
| **Average** | **1.00** | **1.00** |

**Reasoning Time : 1.05s**

---

### PMS

| Query | Target | Results | Time (s) |
|-------|--------|---------|----------|
| Q1 | All risks | Data_Breach, Database_Compromise, Session_Hijacking, System_Takeover | 0.46 |
| Q2 | Critical unmitigated risks | None (all risks mitigated) | 0.30 |
| Q3 | AtRisk without countermeasures | Apache_Server, MySQL_Database, PharmacyWebApp | 0.55 |
| Q4 | Countermeasure coverage | 7 on PharmacyWebApp, 2 on MySQL_Database, 2 on Apache_Server | 0.48 |

| Scenario | Detection Rate | Precision |
|----------|---------------|-----------|
| SQL Injection | 1.00 | 1.00 |
| XSS | 1.00 | 1.00 |
| Command Injection | 1.00 | 1.00 |
| Weak Authentication | 1.00 | 1.00 |
| Session Mgmt. | 1.00 | 1.00 |
| **Average** | **1.00** | **1.00** |

**Reasoning Time : 1.10s**

---

## Tools Required

| Tool | Version |
|------|---------|
| [Protégé](https://protege.stanford.edu/) | 5.6.7 |
| HermiT Reasoner | 1.4.3.456 |
| SWRLAPI + Drools | latest |

---

## Paper

> Meradi C., Aouag S., Beloucif A. (2025).
> *Onto-Security Checks: An Ontology-Based Reasoning Approach
> for Automated Security Verification in Software Architectures.*
> Preprint submitted to Elsevier.

---

## License

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

This work is licensed under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/)
