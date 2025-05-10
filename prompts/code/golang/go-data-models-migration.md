# Go Data Models and PostgreSQL Migrations

## Context
You are an expert Golang developer specializing in database implementations. You need to create Go struct models and migration files for a job board application that connects companies with job seekers, focusing on technology skills.

## Task
Create Go struct models and migration files based on the provided SQL schema. These will serve as the foundation for the database layer of the application.

## Technologies
- Go (Golang)
- pgx for PostgreSQL interaction
- golang-migrate for migrations
- PostgreSQL

## SQL Schema
```sql
-- Companies Table
CREATE TABLE companies (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    careers_page_url VARCHAR(255),
    logo_url VARCHAR(255),
    description TEXT,
    industry VARCHAR(100),
    active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Jobs Table
CREATE TABLE jobs (
    id SERIAL PRIMARY KEY,
    company_id INT REFERENCES companies(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    experience_level VARCHAR(50) NOT NULL, 
    employment_type VARCHAR(50) NOT NULL,
    location VARCHAR(50) NOT NULL,
    work_mode VARCHAR(20) NOT NULL,
    application_url VARCHAR(255) NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    signature VARCHAR(64) UNIQUE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Technologies Table (canonical names)
CREATE TABLE technologies (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    category VARCHAR(50),
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Technology Aliases Table
CREATE TABLE technology_aliases (
    id SERIAL PRIMARY KEY,
    technology_id INT NOT NULL REFERENCES technologies(id) ON DELETE CASCADE,
    alias VARCHAR(100) NOT NULL UNIQUE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

-- Job Technologies Junction Table
CREATE TABLE job_technologies (
    id SERIAL PRIMARY KEY,
    job_id INT REFERENCES jobs(id) ON DELETE CASCADE,
    technology_id INT REFERENCES technologies(id) ON DELETE CASCADE,
    is_primary BOOLEAN DEFAULT FALSE,
    is_required BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    UNIQUE(job_id, technology_id)
);

-- Create Indexes

-- Companies Indexes
CREATE INDEX idx_companies_name ON companies(name);
CREATE INDEX idx_companies_industry ON companies(industry);

-- Jobs Indexes
CREATE INDEX idx_jobs_title_tsvector ON jobs USING GIN (to_tsvector('english', title));
CREATE INDEX idx_jobs_description_tsvector ON jobs USING GIN (to_tsvector('english', description));
CREATE INDEX idx_jobs_location ON jobs(location);
CREATE INDEX idx_active_jobs ON jobs(id) WHERE is_active = TRUE;
CREATE INDEX idx_jobs_work_mode ON jobs(work_mode);
CREATE INDEX idx_jobs_created_at ON jobs(created_at);
CREATE INDEX idx_jobs_company_id ON jobs(company_id);
CREATE UNIQUE INDEX idx_jobs_signature ON jobs(signature);
CREATE INDEX idx_jobs_employment_type ON jobs(employment_type);
CREATE INDEX idx_jobs_experience_level ON jobs(experience_level);

-- Technologies Indexes
CREATE INDEX idx_technologies_name ON technologies(name);
CREATE INDEX idx_technologies_category ON technologies(category);

-- Technology Aliases Indexes
CREATE INDEX idx_technology_aliases_technology_id ON technology_aliases(technology_id);
CREATE INDEX idx_technology_aliases_alias ON technology_aliases(alias);

-- Case-insensitive index for alias searches
CREATE INDEX idx_technology_aliases_lower_alias ON technology_aliases(LOWER(alias));

-- Job Technologies Indexes
CREATE INDEX idx_job_technologies_job_id ON job_technologies(job_id);
CREATE INDEX idx_job_technologies_technology_id ON job_technologies(technology_id);
CREATE INDEX idx_job_technologies_primary ON job_technologies(job_id) WHERE is_primary = TRUE;
```

## Deliverables

1. **Go Struct Models**:
   - Create well-structured Go structs for all entities
   - Include appropriate JSON and SQL tag annotations
   - Define proper struct relationships that reflect database relationships
   - Add comprehensive comments explaining each field and its purpose
   - Implement any custom handling needed for timestamps or specific data types

2. **Migration Setup**:
   - Configuration for golang-migrate
   - Up and down migration scripts that match the provided schema
   - Instructions on how to run migrations

3. **Project Structure**:
   - Define a clean, feature-based structure for models and migrations
   - Follow idiomatic Go package organization

## Guidelines
- Use idiomatic Go naming conventions
- Include proper comments for documentation
- Use appropriate types for all fields
- Structure code for maintainability
- Follow modern Go practices for struct definitions
- Consider the use of nullable fields versus pointers
- Add appropriate JSON tags for API integration