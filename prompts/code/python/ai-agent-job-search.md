## Job Search Automation Agent

## Context

I need to develop an automated job search solution that leverages AI to monitor company career websites for relevant opportunities in Costa Rica and LATAM. The database infrastructure is already in place, including models and repository layer with CRUD operations. Your task is to implement an intelligent agent that will interface with this existing system to scrape job listings and process them according to specific requirements.

## Role
You are an expert Python developer specializing in AI agents and web automation. You have extensive experience with LangChain for building intelligent agents, Playwright for web scraping, and integrating AI capabilities with database systems. Your expertise includes developing clean, maintainable Python applications that follow feature-based architecture principles and best practices for code organization. You're skilled at designing systems that can intelligently parse and process web content, particularly job listings, and integrate smoothly with existing database infrastructure.

## Technical Environment
- **Programming Language**: Python 3.9+
- **Key Frameworks**: LangChain (for AI agent capabilities), Playwright (for web scraping)
- **Database**: Existing implementation with models and repository layer
- **Execution**: On-demand local execution

## Database and Repository Context
- **Schema Location**: [TYPE_HERE]
- **Database Models**: [TYPE_HERE] 
- **Repository Layer**: [TYPE_HERE]

**Important**: You have permission to extend the repository layer by creating additional methods if needed for the agent. Any extensions to the repository layer should follow the same patterns and conventions as the existing codebase. Document any repository extensions you create in the database README.

## Required Functionality

### 1. AI Agent Architecture
Develop an AI agent using LangChain that can:
- Interface with the existing repository layer
- Make decisions about job relevance based on location criteria
- Extract structured data from unstructured job postings
- Manage the orchestration of the entire scraping and processing workflow

### 2. Web Scraping Component
Implement using Playwright to:
- Navigate to and scrape career pages for all active companies
- Handle various website structures and job listing formats
- Extract key information according to job database model
- Operate headlessly in a production environment

### 3. Job Processing Logic
The agent must implement the following workflow:
- Fetch all active companies from the database
- For each company:
  - Visit their careers page via Playwright
  - Identify and extract all job listings
  - For each job listing:
    - Determine if the job is available in Costa Rica or open to LATAM
    - Discard jobs that explicitly exclude Costa Rica
    - Generate a signature field (hash of company ID + job title)
    - Compare against existing jobs in the database matching by signature field
    - For new jobs:
      - Store in the jobs table
      - Extract and store associated technologies in the job technology table
    - For existing jobs:
      - Update the `last_seen_at` timestamp
    - For jobs no longer listed:
      - Mark as inactive in the database

### 4. Location Filtering
- The career page URLs will have location filters pre-selected for Costa Rica/LATAM where possible
- Job locations are clearly stated on the career pages
- No complex pattern matching or keyword detection is required
- Simply check if the location explicitly mentions Costa Rica or LATAM

### 5. Error Handling
- Implement simple error handling with no retries or backoff strategies
- Use logrus for logging errors
- Log any errors that would prevent completing the task
- Continue processing other companies if one fails

## Deliverables
1. **Source Code**:
   - Complete Python implementation of the AI agent
   - Integration with existing repository layer
   - Simple environment configuration file

2. **Documentation**:
   - Comprehensive README.md with:
     - System architecture diagram
     - Component descriptions and interactions
     - Setup and installation instructions
     - Usage instructions for on-demand execution
     - Troubleshooting guide
   - Code documentation with docstrings

## Implementation Guidelines
- Use asynchronous programming where appropriate for performance
- Implement simple rate limiting to avoid overloading target websites
- No authentication or login flows are required for career sites
- Build with extensibility in mind to accommodate future companies
- Focus on successful one-time processing of each company's career page

## Code Quality Requirements
- **Type Annotations**: Use proper Python type hints throughout the codebase for improved maintainability and IDE support
- **Code Organization**: 
  - Follow feature-based clean architecture principles
  - Organize the package structure logically with clear separation of concerns
  - Use domain-driven design concepts where appropriate
- **Code Style**:
  - Balance performance optimizations with code readability
  - Follow PEP 8 and other Python style conventions
  - Include appropriate docstrings for all modules, classes, and functions
- **Best Practices**:
  - Use dependency injection to facilitate testing and component replacement
  - Implement appropriate design patterns where they add value
  - Prefer composition over inheritance
  - Write code that is self-documenting with meaningful variable and function names

## Success Criteria
- The agent successfully identifies and stores relevant job opportunities
- Location filtering correctly identifies jobs available in Costa Rica or LATAM
- Code is maintainable, well-documented, and follows Python best practices
- The implementation demonstrates clean architecture principles with clear separation of concerns
- Type annotations are used consistently throughout the codebase
- Package structure follows logical organization based on features rather than technical layers
- The solution works on a local development environment

## Business Requirements

### Technologies & Job Technologies

This is how the technologies and job technologies table work.

1. First, the job gets saved in the `jobs` table with basic info:
    - Job title: "Golang Developer"
    - Company: Microsoft
    - Description: "Looking for a Golang developer with Docker and AWS experience..."
2. From this job description, we find these technologies:
    - Golang
    - Docker
    - AWS
3. To save them, we need to find out the ID of the technology in the `technologies` table:
    - The `technologies` table is like a dictionary of all tech skills
    - It has entries like "Golang" (ID: 25), "Docker" (ID: 42), and "AWS" (ID: 17)
4. Now, when saving in the `job_technologies` table, we will do:
    - It adds a row connecting Job ID 123 with Technology ID 25 (Golang)
    - It adds another row connecting Job ID 123 with Technology ID 42 (Docker)
    - It adds a third row connecting Job ID 123 with Technology ID 17 (AWS)
    - It marks Golang as the primary technology (setting is_primary = true)

So now this job is connected to three technologies. It's like saying "this job requires these three skills."

Let's say you want to find Golang jobs that also require Docker:

1. Without these special tables:
    - You'd have to search for "Golang" AND "Docker" in the text
    - This might miss jobs that use terms like "Go" instead of "Golang"
    - It might find false matches where "Docker" appears but isn't required
2. With these tables, the search works like this:
    - Find the IDs for "Golang" and "Docker" in the `technologies` table
    - Then use those IDs to find jobs in the `job_technologies` table that have BOTH technologies
    - This gives you a perfect match of jobs requiring both skills

For example, this SQL query would find all Golang + Docker jobs:

```sql
SELECT j.* FROM jobs j
JOIN job_technologies jt1 ON j.id = jt1.job_id
JOIN job_technologies jt2 ON j.id = jt2.job_id
JOIN technologies t1 ON jt1.technology_id = t1.id
JOIN technologies t2 ON jt2.technology_id = t2.id
WHERE t1.name = 'Golang' AND t2.name = 'Docker' AND j.is_active = true;
```

The relationship is like a bridge connecting jobs to the skills they require. This makes searching much more accurate and allows for cool filters like:

- "Show me all React jobs"
- "Show me jobs that use both PostgreSQL and MongoDB"
- "Show me jobs where Go is the main skill required"

This is much better than just searching through text!

Let me explain the `category` and `parent_id` fields in the `technologies` table in simple terms:

### The `category` Field

The `category` field helps organize technologies into groups or types. Think of it like shelves in a library.

For example:

- "Programming Languages" would include Python, JavaScript, Go, Java
- "Databases" would include MongoDB, PostgreSQL, MySQL
- "Frontend Frameworks" would include React, Vue, Angular
- "Cloud Services" would include AWS, Azure, Google Cloud

This helps when:

1. You want to display technologies grouped by category in your website
2. A user wants to filter jobs by a whole category (e.g., "Show me all database jobs")
3. You want to generate reports like "Most in-demand programming languages in Costa Rica"

### The `parent_id` Field

The `parent_id` field creates a family tree relationship between technologies. Some technologies are "children" of other technologies.

For example:

- React is a child of JavaScript
- Django is a child of Python
- AWS Lambda is a child of AWS

This is useful when:

1. A user searches for "JavaScript jobs" - you might want to also include React jobs
2. You want to show related technologies on a job posting
3. You're building recommendation features ("If you know Python, you might also look at Django jobs")

## Real Example

Let's say you have these entries in your `technologies` table:

| id | name | category | parent_id |
|----|------|----------|-----------|
| 1 | JavaScript | Programming Language | NULL |
| 2 | React | Frontend Framework | 1 |
| 3 | Vue | Frontend Framework | 1 |
| 4 | Python | Programming Language | NULL |
| 5 | Django | Web Framework | 4 |

Here:

- JavaScript and Python are top-level technologies (no parent)
- React and Vue are in the "Frontend Framework" category and are children of JavaScript
- Django is in the "Web Framework" category and is a child of Python

This structure helps organize the technologies in a way that makes searching more intelligent and your interface more user-friendly.