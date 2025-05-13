# The Dannyokec AI Task Draft System

## What Is This?
The Dannyokec AI Task Draft System is a step-by-step method that helps you turn your ideas into actionable tasks using AI. Think of it as a bridge between "I have an idea" and "Here's exactly what needs to be done" - using Large Language Models (LLMs) as your thinking partner.

## Why You'll Love This System
- **Nothing gets missed**: By feeding context before intent, the AI understands your environment completely
- **Clear outputs**: Vague ideas become specific, actionable tasks
- **Better collaboration**: Works for solo developers or entire teams
- **Reusable assets**: Create documentation you can use again and again

## The 5-Step Process

### Step 1: Define Your Intent
Create an `intent.md` file that answers:
- What are you trying to accomplish?
- Why is this important?
- What should the final result look like?

**Real Example Intent File Structure:**
```markdown
# Mission
To create contracts and necessary documents for Nigerian technology company dannyokec cyber security and software programming limited (DCSSP). The contracts and documents are for independent contractors and will be used for the company's core subsidiary as the parent company (dannyokec group) wants the subsidiary to quickly build a team while adhering to all necessary documentation and guidelines.

## Guidelines and intent of the contract

### Payment /salary/compensation is on work day basis:
The contractor will receive payment on a daily basis for the days worked on completing or implementing an assigned task. The daily rate will be determined in a separate job rate agreement form...

[Additional sections with contract details]

### Confidentiality and Data Security:
Suggest a confidentiality and data security policy for contractors, employees and partners. I would be using github, figma, notion, google drive, slack, discord, etc for collaboration and execution of tasks. I dont have an idea on this works but a detailed policy would be needed.

## DCSSP ONBOARDING of independent contractor
- Pre-vetting contractors for the project/product at hand(stage one)
[...]
```

### Step 2: Gather Your Context
Collect any relevant documents that help understand your environment:
- Company/team profiles (e.g., Dannyokec Group structure, DCSSP company profile)
- Standard operating procedures (SOPs)
- Existing documentation (e.g., other contracts, HR policies)
- Project constraints (e.g., Nigerian labor laws, tech industry standards)
- Team structures (e.g., how contractors will integrate with existing teams)
- Technical limitations (e.g., collaboration tools, security requirements)

**Pro Tip:** Don't skip this step! The more context you provide, the better the results will be.

### Step 3: Verify Context Understanding
This is where most workflows fail - they don't confirm the AI actually understands the context.

1. Upload all context documents to your LLM session
2. Ask specific questions to test understanding:
   ```
   Based on the company profiles I've shared, what do you understand about Dannyokec Group's structure and how DCSSP fits within it?
   ```
   ```
   What constraints should we consider when drafting contractor agreements for a Nigerian tech company?
   ```
   ```
   Summarize how our current onboarding process works according to the SOPs provided.
   ```

3. Keep asking until you're confident the AI has fully absorbed the context. For example:
   ```
   Based on what you know, what collaboration tools will our contractors need access to?
   ```
   ```
   What security measures are currently in place for protecting company data that contractors might access?
   ```

### Step 4: Review & Refine Your Intent
Now that the AI understands your environment, upload your `intent.md` file with a prompt like:

```
Review the attached intent.md file for creating independent contractor agreements at DCSSP. Based on the company context I've provided:
1. Identify any unclear or ambiguous elements in the payment structure or task fulfillment process
2. Ask questions about the confidentiality policy I need help creating
3. Suggest improvements to the onboarding stages I've outlined
4. Highlight potential conflicts between the contractor agreement and Nigerian labor laws
```

Work with the AI to refine your intent through multiple iterations until:
- All ambiguities are resolved (e.g., clarifying payment dispute procedures)
- All questions are answered (e.g., details about contractor availability system)
- The intent aligns perfectly with your context (e.g., ensuring compliance with local regulations)

**Example Refinement Questions from AI:**
```
1. In the payment section, you mention "the company representative examines the job and requests for correction" - what's the timeline for this review process?

2. For the confidentiality policy, would you like provisions for remote work security, or will contractors only work on-premises?

3. The onboarding stages mention "Test task for initiation" - should the contract specify if these test tasks are paid or unpaid?

4. I notice the intent doesn't address termination conditions beyond task rejection. Would you like to include a section on contract termination procedures?
```

### Step 5: Generate Actionable Tasks
With a crystal-clear intent, now transform it into tasks:

```
Based on our refined independent contractor agreement intent for DCSSP, create a comprehensive task breakdown that:
1. Divides work into logical sections (legal document drafting, system setup, onboarding process development)
2. Lists specific, actionable tasks for each team (legal, HR, IT, management)
3. Identifies dependencies between tasks (e.g., confidentiality policy must be created before onboarding system)
4. Estimates complexity for each task component
5. Tags appropriate team members or roles for each task
```

**Example Task Output:**
```markdown
# DCSSP Independent Contractor System Implementation

## 1. Legal Document Development (Legal Team)
- [ ] Draft base Independent Contractor Agreement template
  - [ ] Payment terms section with daily rate structure
  - [ ] Task assignment and fulfillment procedures
  - [ ] IP rights assignment clause
  - [ ] Dispute resolution process
- [ ] Create Job Rate Agreement Form
- [ ] Develop Confidentiality and Data Security Policy
  - [ ] Platform-specific security protocols (GitHub, Figma, etc.)
  - [ ] Data handling procedures
  - [ ] Breach reporting mechanisms

## 2. System Implementation (IT Team)
- [ ] Develop contractor availability tracking system
  - [ ] Status toggle feature (available, away, sick, unavailable)
  - [ ] Integration with task assignment system
- [ ] Set up secure contractor onboarding workflow
  - [ ] Document collection and verification portal
  - [ ] Automated access provisioning to collaboration tools
- [ ] Create documentation handover system

## 3. Onboarding Process Development (HR Team)
- [ ] Design pre-vetting procedure
  - [ ] Background check process
  - [ ] ID verification workflow
- [ ] Create product/project onboarding materials
  - [ ] Test task templates by role (developer, designer, etc.)
  - [ ] Performance review criteria
- [ ] Develop team introduction protocol
```

## Worked Example: DCSSP Independent Contractor System

Let's walk through exactly how to apply the system to the example intent document for creating contractor agreements for Dannyokec cyber security and software programming limited (DCSSP).

### Step 1: Intent Analysis

Looking at the provided intent.md, we can identify several areas that need refinement:

**Clear Elements:**
- Overall mission to create contractor agreements
- Payment structure based on daily rates
- Four-stage onboarding process

**Areas Needing Clarification:**
- Confidentiality policy (explicitly requested)
- Documentation handover system (mentioned but not detailed)
- Availability tracking system implementation

### Step 2-3: Context Verification (Sample Dialogue)

After uploading company context documents:

**Human:** Based on these documents, what do you understand about our current contractor management process at DCSSP?

**AI:** *From the documents provided, I understand that DCSSP currently handles contractors on an ad-hoc basis without standardized agreements. Your team structure includes product managers who assign tasks directly to contractors. You're currently using GitHub for code management, Figma for design work, and Notion for project tracking, but lack formal access control procedures.*

**Human:** What Nigerian labor laws might impact our independent contractor agreements?

**AI:** *Based on the context provided, I understand that Nigerian labor law distinguishes between employees and independent contractors based on several factors including control, integration, and economic reality tests. Your intent to pay on a per-day basis rather than salary helps establish contractor status, but we should ensure the agreement includes clear language about the contractor's independence to avoid misclassification issues.*

### Step 4: Intent Refinement (Sample Dialogue)

**Human:** *[Uploads intent.md]* Review this intent for our independent contractor system. What needs clarification or improvement?

**AI:** *I have several questions about your intent document:*

*1. In the payment section, you mention negotiating days required for task completion, but don't specify who has final authority if agreement can't be reached. Should this be the company representative?*

*2. For the confidentiality policy you requested, would you prefer:*
   *- A comprehensive single policy document*
   *- Platform-specific guidelines for each tool (GitHub, Figma, etc.)*
   *- Both approaches combined?*

*3. The "documentation handover" system is mentioned but not detailed. What specific requirements do you have for this system?*

*4. For the availability tracking system, will this be implemented in an existing tool or require custom development?*

**Human:** *[Provides answers to clarify these points]*

### Step 5: Task Generation Output

```markdown
# DCSSP Independent Contractor System Implementation Plan

## 1. Legal Documentation (Legal Team - 2 weeks)
- [ ] Draft Master Independent Contractor Agreement
  - [ ] Daily rate payment structure with negotiation provisions
  - [ ] Task acceptance/rejection procedure with 24-hour timeline
  - [ ] IP rights assignment clause
  - [ ] Dispute resolution process (dialogue → arbitration → litigation)
- [ ] Create Job Rate Agreement Form template
- [ ] Develop Equipment Leasing Policy for contractors
- [ ] Create Confidentiality and Data Security Policy
  - [ ] General principles and requirements
  - [ ] Tool-specific security protocols:
    - [ ] GitHub repository access controls
    - [ ] Figma file permissions structure
    - [ ] Notion workspace security settings
    - [ ] Google Drive document protection
    - [ ] Slack/Discord communication guidelines

## 2. Technical Systems Implementation (IT Team - 3 weeks)
- [ ] Develop contractor availability tracking system
  - [ ] Status toggle feature (available, away, sick, unavailable)
  - [ ] Status API for integration with task assignment
  - [ ] Reporting interface for management
- [ ] Set up secure document collection portal
  - [ ] Government ID verification system
  - [ ] Secure document storage with appropriate access controls
- [ ] Create documentation handover system
  - [ ] Template for deliverable documentation
  - [ ] Automated archiving of completed work
  - [ ] Knowledge transfer checklist

## 3. Process Development (HR/Operations - 2 weeks)
- [ ] Design 4-stage onboarding workflow
  - [ ] Pre-vetting stage documentation and checklist
  - [ ] Product/project onboarding materials by role
  - [ ] Post-vetting contractor database
  - [ ] Collaboration space access provisioning process
- [ ] Create task assignment templates for:
  - [ ] GitHub issue assignment (developers)
  - [ ] Figma task assignment (designers)
  - [ ] Project management task creation (other roles)
- [ ] Develop performance review criteria and templates

## 4. Training and Implementation (All Teams - 1 week)
- [ ] Create training materials for company representatives
- [ ] Develop contractor onboarding guides
- [ ] Conduct pilot run with 2-3 test contractors
- [ ] Gather feedback and refine processes
- [ ] Full system launch
```

## Implementation Tips

### For Solo Entrepreneurs
- Start with just the sections you need most urgently
- Use the system to create your first SOPs and playbooks
- Build your documentation library as you grow

### For Growing Companies
- Use the system to standardize processes across departments
- Create intent templates for recurring document types
- Build a library of refined intents that reflect your company's values

### For Different Project Types
- **HR Systems**: Focus on compliance, scalability, and user experience
- **Legal Frameworks**: Emphasize jurisdiction-specific requirements and risk management
- **Technical Infrastructure**: Highlight security, integration points, and maintenance

## Getting Started Right Now
1. Look at your current project needs and choose one that's unclear
2. Create an intent.md file following the example structure
3. Gather relevant context documents about your organization
4. Start a new LLM chat session
5. Follow the 5-step process, keeping notes of important clarifications
6. Use the generated tasks to drive implementation

Remember: The quality of your tasks depends on how thoroughly you refine your intent. Invest time in the clarification process!

---

*The Dannyokec AI Task Draft System transforms vague ideas into actionable plans through structured AI collaboration. Adapt this framework to your specific needs and watch your productivity transform.*
