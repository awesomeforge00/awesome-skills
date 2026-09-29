---
name: job-search-and-apply
description: Find relevant jobs from a provided resume and explicit preferences, assess core fit, and return companies with direct application links for the user to review and apply themselves.
---

# Job Search and Apply

Find relevant openings using the user's resume and stated preferences. Return a shortlist of companies, roles, and application links for the user to review and apply themselves. Never submit an application on the user's behalf.

## Start each run

Before searching, establish the current run's inputs:

- The resume to use. Read it only for this job search; do not edit or redistribute it.
- Explicit preferences and exclusions, including target roles, locations, remote or hybrid requirements, seniority, industries, compensation, and work authorization or sponsorship where relevant. Ask rather than infer missing preferences or sensitive details.
- Relevant job boards and company career sites to search. Check LinkedIn, Indeed, Naukri, Glassdoor, Cutshort, Wellfound, Foundit, Instahyre, Y Combinator Jobs, Remote OK, and employer career pages where they have relevant listings and are accessible with authorized tools. Add other role- or region-specific platforms when useful.

If required preferences are missing, ask focused questions before searching. Do not ask for passwords, one-time codes, or other login secrets.

## Search and assess jobs

1. Search the selected job boards and employer career sites using role titles, skills, locations, and other criteria from the resume and explicit preferences. Choose additional platforms based on relevance to the target role and region.
2. Deduplicate results across sources using the job URL or listing ID; when those are unavailable, compare employer, title, and location.
3. Apply explicit exclusions and must-have requirements first. Exclude a role that fails a must-have or hard constraint. For other roles, assess how well the responsibilities and essential qualifications match; missing preferred or nice-to-have qualifications do not automatically disqualify a role. Do not require a 100% match.
4. Do not infer qualifications or claim experience absent from the resume or user-provided information. If eligibility cannot be assessed due to missing information, flag the uncertainty rather than presenting the role as a confirmed fit.
5. Prefer the employer's official application page as the apply link. If only a job-board listing is available, provide that link and identify the source. Never describe a search as exhaustive: state which sources were checked and note any access or result limits.

## Present results

- List every qualifying company and role found within the searched sources and run scope. Include multiple qualifying roles for the same company.
- For each role, provide company, title, location, source platform, direct apply link, a concise core-fit rationale, and any notable missing preferred qualifications or unresolved eligibility questions.
- Group or sort results for easy comparison, and clearly separate roles that fail a must-have or remain uncertain if mentioning them is useful.
- State the platforms checked and any limitations. Do not imply that the list covers every open job or company.
- The user reviews the results and completes applications themselves. Do not fill forms, upload resumes, submit applications, or maintain an application-submission tracker.

## Save the shortlist

- At the end of each run, save every qualifying role listed in the results to a CSV file. Include a header row and these columns: `company`, `job_title`, `location`, `source_platform`, `apply_url`, `core_fit`, `missing_preferred_qualifications`, `eligibility_notes`.
- Use a private destination selected by the user. If no destination is provided, ask before writing; do not save job-search results in this skill repository or another shared project by default.
- Include one row per listed role, preserve the apply URL, leave unknown optional values blank, and quote/escape CSV fields correctly. If there are no qualifying roles, save a header-only CSV.
- Never overwrite an existing file without the user's explicit permission. Report the path of the completed CSV and any rows that could not be saved.

## Search safeguards

- Follow platform rules and respect access controls, rate limits, and site restrictions.
- Never bypass CAPTCHA, bot detection, access controls, or other safeguards. If sign-in or verification blocks access, report the limitation and continue with other accessible sources.
- If a platform cannot be searched with available tools, say so and do not imply it was checked.