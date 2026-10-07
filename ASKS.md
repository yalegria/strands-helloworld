# Strands Decider CLI Reference & Experimentation Playbook

This file contains practical examples for testing the `strands-decider` model locally. Use these to experiment with classification accuracy, threshold tuning, and multi-choice routing.

> **Tip:** The model `StrandsAgents/strands-decider-2B-hobson-v19` must be downloaded locally for these commands to run.

---

## 🛠️ 1. Helpdesk & IT Ticket Routing
Standard routing scenarios to test how well the model distinguishes between infrastructure, software application, and hardware issues.

### Example A: Database Performance Issue
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "The production PostgreSQL cluster is showing high CPU spikes and queries are taking over 5000ms to execute during peak traffic." \
  --choice "Target Team=DevOps,Frontend,SecOps,Helpdesk"
```

### Example B: Workplace Hardware Request
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "My laptop screen is flickering constantly when connected to the external monitor docking station." \
  --choice "Target Team=DevOps,Frontend,SecOps,Helpdesk"
```

---

## 🚨 2. Security Incident Triage (SecOps)
Use these to evaluate risk or decide whether a security event needs immediate human escalation.

### Example A: Suspected Phishing
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "An employee reported an unexpected email claiming to be from HR requesting immediate verification of banking details via an external Microsoft Form link." \
  --choice "Severity Level=Low Risk,Medium Risk,High Escalation Required"
```

### Example B: Port Scanning Alert
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "Internal IDS detected a sequential IP sweep scanning ports 22, 80, and 443 originating from an unauthorized IoT device on the guest Wi-Fi network." \
  --choice "Severity Level=Low Risk,Medium Risk,High Escalation Required"
```

---

## 📊 3. System Logs & Alert Analysis
Simulate automated monitoring tools analyzing server logs to determine system health states.

### Example A: Out of Memory (OOM) Crash
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "kernel: [12948.20] Out of memory: Kill process 4012 (node) score 842 or sacrifice child. Killed process 4012." \
  --choice "System Health=Healthy,Degraded,Critical Failure"
```

### Example B: Minor Warnings
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "Nginx logs: 2026/10/07 11:14:02 [warn] 2012#0: 2000 worker_connections are not enough while connecting to upstream." \
  --choice "System Health=Healthy,Degraded,Critical Failure"
```

---

## 💳 4. Customer & Billing Operations
Scenarios testing language subtleties where financial transactions or business retention workflows are triggered.

### Example A: Refund Request
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "I was accidentally billed twice for my premium subscription this month. I need a chargeback or a credit applied to my account immediately." \
  --choice "Action Item=Issue Refund,Account Cancellation,Feature Request,Password Reset"
```

### Example B: Churn Risk / Account Closing
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "Your software is too expensive for our small team. Please close my organization profile at the end of our current billing cycle." \
  --choice "Action Item=Issue Refund,Account Cancellation,Feature Request,Password Reset"
```

---

## 🧪 Experimentation Tasks: Try This Next!
1. **The Overlap Test:** Try running a ticket that mentions both a database crash and a security breach. Watch how the confidence score drops, indicating the model sees an ambiguous edge case.
2. **The Synonym Test:** Change the word `DevOps` to `SysAdmin` or `Cloud Engineering` in the `--choice` parameters to see if the model adapts to your internal nomenclature dynamically.

## 🚚 5. Logistics & Supply Chain Management
Testing how the model triages real-world shipping errors, delays, and physical inventory disruptions.

### Example A: Damaged Freight Arrival
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "Pallet #402 arrived at the loading dock with broken shrink-wrap. Three cases of liquid product have ruptured, soaking the surrounding cardboard boxes." \
  --choice "Warehouse Action=Refuse Shipment,Accept with Exception,Route to Standard Inventory"
```

### Example B: Custom Clearance Hold
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "The container ship has docked, but the manifest is missing the commercial invoice required for importing the electronic components." \
  --choice "Warehouse Action=Refuse Shipment,Accept with Exception,Route to Standard Inventory"
```

---

## 🏥 6. Healthcare & Medical Clinic Triage
Simulating a front-desk check-in system or automated medical intake line to determine priority or dispatching.

### Example A: Acute Physical Symptom
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "Patient reports sudden onset of severe lower abdominal pain accompanied by nausea and a fever of 101.4 degrees." \
  --choice "Urgency Level=Routine Appointment,Urgent Care Walk-In,Emergency Room Escalation"
```

### Example B: Prescription Refill Request
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "I just need a renewal on my standard daily blood pressure medication. I have one pill left and my pharmacy says they need your approval." \
  --choice "Urgency Level=Routine Appointment,Urgent Care Walk-In,Emergency Room Escalation"
```

---

## 🏢 7. Property Management & Facilities Maintenance
Scenarios for managing apartment complexes, commercial real estate, or office environments.

### Example A: Active Infrastructure Hazard
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "The main water pipe in the building's basement has cracked and water is rapidly pooling near the electrical breakers." \
  --choice "Dispatch Type=Scheduled Handyman,Emergency Plumber,Janitorial Cleaning"
```

### Example B: Cosmetic Issue
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "The drywall in the third-floor hallway has a scuff mark and some chipped paint from when the new tenants moved their furniture in." \
  --choice "Dispatch Type=Scheduled Handyman,Emergency Plumber,Janitorial Cleaning"
```

---

## ✈️ 8. Hospitality & Travel Booking Operations
Testing nuance in customer sentiment and policy enforcement for hotels or airlines.

### Example A: Last-Minute Force Majeure
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "My flight was completely canceled due to a category 4 hurricane, and there are no other flights leaving this week. I need to cancel my hotel stay tonight." \
  --choice "Policy Applied=Enforce Standard Cancellation Fee,Issue Full Refund Exception,Offer Future Travel Credit"
```

### Example B: Simple Change of Mind
```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "We decided to stay an extra night at the beach instead, so we won't be checking into your property until tomorrow afternoon." \
  --choice "Policy Applied=Enforce Standard Cancellation Fee,Issue Full Refund Exception,Offer Future Travel Credit"
```
