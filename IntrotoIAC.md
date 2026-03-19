# TryHackMe Room Writeup — Intro to IaC

---

## The Concept

Key characteristics of Infrastructure as Code:

- **Versioning** — IaC is self-documenting, allowing infrastructure changes to be tracked and recorded. It also allows rollback to the last known working version if issues arise.
- **Repeatable** — The same code/configuration can be used to set up identical infrastructures across multiple environments with the push of a button.
- **Scalable** — Infrastructure resources can be increased or decreased with ease to meet demand.

**Question Answers:**
- First answer: **Repeatable**
- Second answer: **Versionable**
- Third answer: **Scalable**

---

## The Tools Part 1

Common IaC tools include: **Terraform, CloudFormation, Google Cloud Deployment Manager, Ansible, Puppet, Chef, SaltStack, and Pulumi**.

### Declarative

- Focuses on **what** the infrastructure should look like, rather than **how** to get there.
- The tool considers where you currently are and provides instructions to reach the desired state (the "X point" on the map).

### Imperative

- Focuses on **how** — defining specific commands to be run to achieve the desired state.
- Gives the user more control, allowing them to specify exactly how the infrastructure is provisioned and managed.
- Examples: **Chef** is the best example of an imperative tool; **SaltStack** and **Ansible** also support imperative programming.

### Agent-Based vs Agentless

- **Agent-based** tools require an agent to be installed on the managed server. The agent runs in the background and acts as a communication channel between the IaC tool and the resources being managed.
- Agent-based tools include: **Puppet, Chef, and SaltStack**.

**Question Answer:**
- **Declarative** — considers where you are and gives instructions to reach the desired state.

---

## The Tools Part 2

**Immutable vs Mutable infrastructure** refers to whether or not changes can be made to an existing infrastructure after it has been provisioned.

**Question Answer:**
- Follow through the site to get the flag: `THM{l4b_C0mpl3x_co0rds}`

---

## Infrastructure as Code Lifecycle

The IaC lifecycle helps us understand that infrastructure as code involves both **continual** and **repeatable** processes.

**Question Answers:**
- The **repeatable** phase is the first answer.
- **Continual** phases ensure best practices throughout infrastructure development and management.
- The **rollback** phase is the third answer — the Monitoring/Maintenance continual phase can trigger it.

---

## Virtualisation & IaC

When discussing virtualisation levels, we are essentially talking about **what** is being virtualised.

| Scenario | Answer |
|---|---|
| OS-level virtualisation for lightweight, rapid deployment | **Containerisation** |
| Multiple OS running on a single machine | **Hypervisor** |
| Isolating resource-intensive features from other components | **Resource Isolation** |
| Container orchestration for rapid scaling | **Kubernetes** |

---

## On-Prem IaC vs Cloud-Based IaC

Key differences between on-prem and cloud-based IaC span five categories: **Location, Technology, Resources, Scalability, and Cost**.

**Question Answers:**
- The underlying infrastructure in a cloud environment is handled by the **cloud service provider**.
- On-prem infrastructure struggles with **scalability** due to hardware limitations when facing increased traffic.

---

## The Final Push

Follow through the site to complete the exercise.

> Flag: `THM{1Nfr4StrUctUr3_Pr0}`
