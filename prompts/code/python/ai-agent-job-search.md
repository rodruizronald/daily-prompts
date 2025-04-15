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
