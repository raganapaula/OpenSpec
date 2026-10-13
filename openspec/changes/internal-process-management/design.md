# Design

## Overview

The design centers on a process-centric data model that keeps process definitions, stage details, and future-state documentation clearly separated while preserving traceability across versions. The system supports both operational documentation and process improvement planning without mixing the current and future versions into a single record.

## Core Entities

### Process
Represents the organizational workflow being managed. It contains the process identity, owner, area, objective, and lifecycle status.

### Stage
Represents a discrete unit of work within a process. Each stage is ordered and associated with a responsible role or user. It may describe manual tasks, system work, decisions, or approvals.

### Responsibility
Defines who or what owns a stage or process. This can be a role, team, or individual, depending on governance needs.

### Input and Output
Capture the artifacts, information, or services consumed or produced by the process. These values help explain the relationship between the process and the broader organizational flow.

### Process Version
Captures either AS-IS or TO-BE states. It links a process to its documentation and preserves the historical evolution of the workflow.

## Data Model

- Process: id, name, code, department, objective, owner, status, created_at, updated_at
- Stage: id, process_id, order, name, description, type, responsible_id, estimated_duration, notes
- Responsibility: id, name, role, department, email, type
- Input: id, process_id, name, description, source, type, required
- Output: id, process_id, name, description, destination, type, required
- ProcessVersion: id, process_id, version_type (AS-IS or TO-BE), title, description, created_by, created_at
- ImprovementRecord: id, process_id, version_id, summary, priority, status, impact

## Functional Architecture

### 1. Process registry module
This module allows users to create, update, view, and archive process records. It keeps the canonical process description and connects to all associated stages and documentation.

### 2. Procedure definition module
This module manages the ordered sequence of stages and associated responsibilities. It supports updates to stage logic and identifies where the process is manual or automated.

### 3. Input/output registry
This module maintains the artifacts and data flowing into and out of the process. This improves the ability to understand dependencies and operational boundaries.

### 4. AS-IS / TO-BE documentation module
This module stores the two primary process states. AS-IS represents the actual workflow as currently operated, while TO-BE captures the target future state after improvement decisions.

### 5. Comparison and improvement module
This module provides the ability to compare the current and target process and supports change analysis, bottleneck review, and improvement planning.

## User Flow

1. Create or open a process.
2. Define the objective and owner.
3. Add stages in the correct order.
4. Record inputs, outputs, and responsible entities.
5. Document the AS-IS process state.
6. Define an improved TO-BE version.
7. Compare both states and review gaps.
8. Save the version history and track change decisions.

## Design Considerations

- Separation of concerns: process, stages, and documentation are independent and linked through references.
- Auditability: the system keeps a version trail so past states remain available.
- Clarity: the distinction between AS-IS and TO-BE prevents confusion between current operations and desired transformation.
- Extendability: the model can grow to include indicators, approvals, and process metrics without major redesign.

## Non-Goals

- Automated workflow execution engine
- Complex BPMN engine without process documentation support
- Full enterprise workflow orchestration beyond the organizational process management scope
