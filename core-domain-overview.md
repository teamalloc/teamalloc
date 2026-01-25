# TeamAlloc – Core Domain Overview

This document provides a **semantic, domain-oriented overview** of the core entities in **TeamAlloc**.  
It explains their meaning, function, relationships, and typical use cases from a planning and management perspective.

The focus is on **what the entities represent conceptually**, not on technical implementation details.

---

## 1. Tenant

### Meaning

A **Tenant** represents an organizational boundary, such as a company, legal entity, or independent business unit.

### Function

- Provides strict data isolation
- Defines the scope for users, roles, permissions, and all planning data
- Enables TeamAlloc to be used by multiple organizations or divisions in parallel

### Typical Use Cases

- Separate planning for different subsidiaries
- Independent access control per organizational unit
- Shared platform with clear separation of responsibilities

---

## 2. Person and Tenant Membership

### Meaning

A **Person** represents a real human being, identified globally through an identity provider.  
A **Tenant Membership** represents that person’s role and permissions within a specific tenant.

### Function

- Allows one person to work in multiple organizational contexts
- Serves as the anchor for authorization, auditing, and responsibility
- Separates identity from organizational context

### Typical Use Cases

- Managers involved in multiple business units
- External consultants invited into a specific tenant
- Auditable, tenant-specific responsibility for planning changes

---

## 3. Team

### Meaning

A **Team** represents an organizational unit such as a department, squad, or functional group.

Teams are **structural constructs**, not capacity holders.

### Function

- Organize resources by organizational affiliation
- Support reporting and management views
- Provide context for resource assignments

### Relations

- A team can have many resources assigned over time
- A resource can belong to multiple teams at different or overlapping periods

### Typical Use Cases

- Department-based capacity analysis
- Organizational reporting
- Structuring planning views by responsibility areas

---

## 4. Resource

### Meaning

A **Resource** represents a unit of planning capacity.  
It may correspond to an individual person, a role-based position, or an external capacity pool.

The key characteristic of a resource is that it provides **allocatable capacity and cost**.

### Function

- Supplies capacity measured in FTE and person-days
- Carries cost information
- Acts as the basic unit for all allocation decisions

### Relations

- Assigned to teams over time (Team Assignment)
- Assigned to projects over time (Project Resource Assignment)

### Typical Use Cases

- Workforce capacity planning
- Role-based allocation analysis
- Cost projections based on planned utilization

## 4a. Resource Role

### Meaning

A **Resource Role** represents a a role of a resource.  
A tenant can define multiple types for a resource role, like "software engineer" or "business analyst".

A resource has a default role, but can be assigned to other roles at a specific project and over time.

### Function

- Helps the AI buddy to understand the role of a resource

### Relations

- Assigned to resources over time (Resource Assignment)

### Typical Use Cases

- Workforce capacity planning
- Role-based allocation analysis

---

## 5. Project

### Meaning

A **Project** represents a planned initiative that consumes capacity over time to achieve a defined outcome.

Projects are treated as **planning constructs**, not execution artifacts.

### Function

- Define demand in terms of effort, timeline, and dependencies
- Serve as the primary driver of resource allocation
- Enable portfolio-level planning and prioritization

### Relations

- Assigned resources over time
- Role-based demand definitions
- Dependencies on other projects (finish-to-start)
- Included in scenarios as snapshot objects

### Typical Use Cases

- Portfolio planning
- Evaluation of feasibility for new initiatives
- Understanding resource impact of strategic decisions

---

## 6. Project Dependencies

### Meaning

A **Project Dependency** expresses a planning constraint between two projects.

Currently supported: **Finish-to-Start**, meaning one project may only start after another is planned to end.

### Function

- Enforce realistic sequencing in planning
- Prevent inconsistent timelines
- Support scenario comparison with valid assumptions

### Typical Use Cases

- Program planning with phased initiatives
- Dependency-aware scenario modeling
- Impact analysis of schedule changes

---

## 7. Scenario

### Meaning

A **Scenario** represents a frozen planning alternative captured at a specific point in time.

It answers the question:  
*“What would our planning look like under these assumptions?”*

### Function

- Preserve alternative planning realities
- Enable comparison without changing the baseline
- Provide transparency for assumptions and trade-offs

### Relations

- Contains snapshots of projects
- Includes resource allocations, team involvement, and dependencies
- Can be reused across multiple plans

### Typical Use Cases

- “What-if” analyses
- Management decision preparation
- Risk and impact assessments

---

## 8. Plan

### Meaning

A **Plan** is a curated collection of scenarios intended for structured decision-making.

Plans do not introduce new planning data; they organize existing scenarios.

### Function

- Bundle scenarios for comparison
- Provide context for management discussions
- Serve as a decision framework

### Relations

- A plan contains one or more scenarios
- A scenario may appear in multiple plans

### Typical Use Cases

- Executive decision rounds
- Portfolio approval processes
- Budget and strategy alignment

---

## 9. External Systems and Integrations

### Meaning

External systems represent third-party or in-house applications that exchange data with TeamAlloc.

### Function

- Enable reuse of master data from HR, ERP, or project systems
- Allow external systems to reference TeamAlloc entities using their own identifiers
- Support enterprise-wide planning workflows

### Attributes & Metadata

**Attributes** allow extending any core entity (Projects, Resources, etc.) with custom fields. They serve as the primary
bridge for external data that doesn't fit the core planning model.

- **Meaning**: Arbitrary metadata associated with a domain entity (e.g., "Skill Level", "Cost Center", "Priority").
- **Function**: Store external identifiers, categorization tags, or specific planning parameters.
- **Typical Use Cases**:
    - Mapping Resources to external HR Cost Centers.
    - Synchronizing Project "Tags" from a Project Management tool.
    - Storing technical skills for AI-assisted resource matching.

---

## 10. Onboarding and Invites

### Meaning

**Invites** are the secure gateway for bringing new users into a specific organizational context (Tenant).

### Function

- Controlled provisioning: Users can only join a tenant if explicitly invited.
- Identity verification: Integration with **Keycloak** ensures that users are authenticated against trusted identity
  providers.
- Role-based bootstrapping: Invites can automatically grant initial roles and permissions upon acceptance.

### Typical Use Cases

- Inviting a new department head to manage their team's capacity.
- Onboarding external consultants with restricted, temporary access.
- Ensuring only users with a specific corporate email domain can accept an invitation.

---

## 11. Financial Data Protection (RBAC:FINANCE)

### Meaning

**RBAC:FINANCE** is a specialized security layer that protects sensitive financial information within the planning
model.

### Function

- Fine-grained field-level security: Separates the ability to see a project/resource from the ability to see its cost or
  revenue.
- Need-to-know enforcement: Ensures that salary-related or revenue-sensitive data is only visible to authorized
  personnel (e.g., Finance Managers or Executives).

### Typical Use Cases

- A project manager can see resource assignments but not the individual hourly rates of the resources.
- A resource planner can see capacity utilization without being exposed to revenue projections.
- Central finance departments maintaining full visibility while operational leads focus on effort and timelines.

---

## 12. AI as a Planning Assistant

### Meaning

AI in TeamAlloc acts as a **decision-support mechanism**, not as an autonomous planner.

### Function

- Identify potential capacity conflicts
- Suggest alternative allocations or scenarios
- Support exploratory planning and sensitivity analysis

### Typical Use Cases

- Early detection of overload risks
- Faster exploration of planning alternatives
- Decision support for complex trade-offs

---

## Summary

TeamAlloc’s core entities form a **coherent planning model** that connects people, projects, and organizational
structures.

Together, they enable organizations to:

- plan realistically,
- compare alternatives transparently,
- integrate existing systems using flexible **Attributes**,
- secure sensitive data with **RBAC:FINANCE**,
- and make informed, forward-looking decisions.

TeamAlloc is designed to support **thinking before acting**—and to make planning a strategic advantage rather than a
reactive necessity.
