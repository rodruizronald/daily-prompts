# Advanced Query Capabilities for Go Repository Pattern

## Context
You are an expert Golang developer finalizing the database layer for a job board application. The basic repository pattern and CRUD operations have been implemented, and now you need to add advanced query capabilities to support complex filtering, sorting, pagination, and search.

## Task
Extend the repository implementations with advanced query capabilities to support the job search functionality, including text search, filtering by multiple criteria, and efficient pagination.

## Technologies
- Go (Golang)
- pgx for PostgreSQL interaction
- PostgreSQL (with text search capabilities)

## Deliverables

1. **Filter System**:
   - Design a flexible filter struct for job searches
   - Implement filters for:
     * Multiple experience levels
     * Employment types
     * Locations
     * Work modes (remote, hybrid, on-site)
     * Company industries
     * Required and primary technologies
   - Allow for combining multiple filters in a single query

2. **Text Search Implementation**:
   - Leverage PostgreSQL's text search capabilities using the GIN indexes
   - Implement search across job titles and descriptions
   - Support search within technology names and aliases
   - Create a relevance-based ranking system

3. **Pagination & Sorting**:
   - Implement offset and cursor-based pagination options
   - Support customizable page sizes
   - Create efficient total count handling
   - Support sorting by multiple fields with direction control

4. **Technology Relationship Handling**:
   - Implement efficient filtering by required technologies
   - Support filtering by primary technologies
   - Create methods to fetch jobs with their associated technologies
   - Optimize technology-related queries for performance

5. **Query Optimization**:
   - Implement efficient query building
   - Use appropriate PostgreSQL features for performance
   - Add methods for common query patterns
   - Design query patterns that efficiently use the available indexes

## Guidelines
- Focus on performance for search and filter operations
- Create a clean, reusable approach to building complex queries
- Design flexible structures that can be extended with new filters in the future
- Use efficient SQL constructs to minimize database load
- Implement proper error handling for all operations
- Document your code with comprehensive comments explaining approach and optimizations
- Structure the code using a feature-based organization