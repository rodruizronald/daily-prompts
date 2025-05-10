# Go Repository Pattern Implementation

## Context
You are an expert Golang developer implementing the database access layer for a job board application. You have already created the Go struct models based on the schema, and now need to implement repository interfaces and their concrete implementations.

## Task
Create repository interfaces and implementations for all entities in the job board application, with a focus on clean design and proper error handling.

## Technologies
- Go (Golang)
- pgx for PostgreSQL interaction
- PostgreSQL

## Database Schema Overview
A job board application with tables for:
- companies (basic company info)
- jobs (job listings with details)
- technologies (tech skills like "Go", "PostgreSQL")
- technology_aliases (alternative names for technologies)
- job_technologies (junction table linking jobs to required technologies)

## Deliverables

1. **Repository Interfaces**:
   - Define clear interfaces for each entity (Company, Job, Technology, TechnologyAlias, JobTechnology)
   - Include all basic CRUD operations
   - Define methods for common queries based on indexed fields
   - Structure interfaces to support dependency injection

2. **Repository Implementations**:
   - Create concrete implementations of all repository interfaces
   - Implement proper error handling and SQL query construction
   - Ensure efficient connection handling with pgx
   - Add proper transaction support for operations across multiple tables
   - Include comprehensive comments explaining your implementation choices

3. **Connection Management**:
   - Create a connection pool configuration
   - Implement clean dependency injection for the database connection
   - Define a transaction manager to handle multi-table operations

4. **Error Handling**:
   - Define custom error types for database operations
   - Implement consistent error handling patterns
   - Ensure proper handling of SQL-specific errors

## Guidelines
- Follow idiomatic Go patterns for interfaces and implementations
- Use context.Context appropriately for all database operations
- Implement proper transaction handling for operations that modify multiple tables
- Use prepared statements where appropriate for security and performance
- Structure code using a feature-based organization
- Add comprehensive comments explaining implementation decisions