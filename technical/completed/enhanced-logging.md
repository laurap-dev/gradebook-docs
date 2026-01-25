# Enhanced Logging & Error Reporting

## Overview

This document outlines the enhanced logging feature and improved error reporting for the GradeBook system.
The goal is to provide:
1.  **Stage 1: Performance Logging**: Detailed timing metrics for operations.
2.  **Stage 2: Enhanced Error Reporting**: Better diagnostics in admin emails, specifically "Total Time" and "Action Name".

## Stage 1: Timing Logging (Implemented)

### Requirements
- **Scope**: Client-driven applications that pass `operationId` to `logMenuEvent`. Note that not all entry points have been updated yet (e.g., some pageLoad functions); duration tracking applies only to those that explicitly pass `operationId`.
- **Events**: Track start and end of operations via `logMenuEvent`.
- **Duration**: Capture "Total Time" for both successes and failures.
- **Format**: Human-readable (e.g., `45s`, `1 min 30s`).

### Implementation Strategy
- **Mechanism**:
  - Update `logMenuEvent` to accept `operationId`.
  - On `eventType: 'started'`, store start time in `CacheService.getUserCache()` key `op_start_<operationId>`.
  - On `eventType: 'completed'`, retrieval start time, calculate duration, and log it.
  - Provide a helper `getOperationDuration(operationId)` to fetch current elapsed time (useful for constructing error responses before the operation technically "completes").

## Stage 2: Enhanced Error Emails (Implemented)

### Requirements
- **Start**: Implement immediately following Stage 1.
- **Content**: Error emails (sent via `_init/error`) must include:
  - **Action Name**: Which specific action/menu function failed (Critical).
  - **Total Time**: Execution duration up to the error.
  - **Stack Trace**: Formatted stack trace.
  - **Diagnostics**: Other beneficial details.

### Implementation Strategy
- **Error Data Flow**:
  1.  Server function catches error.
  2.  Server calculates/retrieves duration via `getDuration(operationId)`.
  3.  Server includes `duration` and `actionName` in the returned error object.
  4.  Client receives error, displays it, and (optionally) sends Error Report.
  5.  Client includes these fields in the report payload.
  6.  `sendErrorReportToSupport` (Server) receives payload.
  7.  `createErrorReportEmail` (Formatting) renders these fields prominently.