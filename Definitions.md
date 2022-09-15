# Team Definitions
| Term | Definition |
| ---- | ---------- |
| Epic | Project phase or product feature. Should include work completed across multiple work iterations. Equal to one release iteration. User Story Work with a deliverable artifact which impacts one of the users defined below. Should be completed within one work iteration. <br> Will generally follow the format "As a <role>, «the requirement to satisfy> so that \<impact\>." |
| Task | Work with no deliverable artifact. Should be completed within one work iteration. Sub-Task Subset of work attached to either a User Story or a Task. All subsets of work MUST be completed within one work iteration. Should not be used for work that needs to be estimated, use Tasks instead. |
| Risk | External dependency that has been identified as a risk to successful completion of the sprint or project. A Risk could be an issue submitted to the Service Desk, Confluence being down, information expected from the product owner, or any other external dependency. Assign the issue to a person as an indication that there is a point of contact trying to minimize the blocker/risk. Set the Priority to Blocker when it becomes an actual blocker to the progress of the project. Do not estimate Risk issues. |
| Static Review | Review of documentation or source code that does not require execution of the tool |
| Dynamic Review | Review of binary artifacts related to work completed, executing in the development or operational environment |
| Work in Progress (WIP) | Number of tasks in progress at any time. Limit Work in Progress to one JIRA issue per team member. |

# Definition of Done
### Release Iteration
- Team technical lead verifies that dev branch(es) have most current work and master branch has the most recent release iteration.
- Team technical lead merges to the master branch
- Each stakeholder contributes to the Lessons Learned document
- Appropriate documentation (including portion markings)

### Work Iteration
- Development team lead or release manager creates a release candidate ready for functional evaluation
- Development team will complete next work iteration's planning session

[comment - per RAGO docs]: <> (I'd like to have a mini retro/sync at the end of the work iterations even if if's not a full "sprint planning"?)

### JIRA issue
- Original developer creates a review candidate ready for peer review using the
"Development Workflow" defined below
- Secondary developer peer reviews all work submitted by the original developer
- Senior developer reviews all work by the original developer
- Unit tests writtten and passed
- Appropriate Documentation
  
# Definition of Ready
### GitHub Issue
- Issue is written in user story format (as applicable according to the issue template)
- The issue meets the INVEST criteria
    - I - Independent - the issue can be developed independently of other issues; eg changes in one user story does not effect another
    - N - Negotiable - it is up to the team to determine how to implement them; there are no rigidly fixed workflows
    - V - Valuable - It adds identifiable value from the perspective of the end user
    - E - Estimable - The issue provides enough info to allow the team to estimate it
    - S - Small (enough) to be developed and tested
    - T - Testable - Testing is possible based on AC and Definition of Done
- Acceptance Criteria included
- Any amplifying information is included
  
### Acceptance Criteria
All new tickets must be submitted in this format.

```markdown
### Estimate: X (For GitHub only, use the "Weight" section in GitLab to track estimates)
# Objective

- [ ] Task 1
- [ ] Task 2

# Acceptance Criteria
- [ ] Meets Definition of Done
- [ ] AC 2
- [ ] AC 3
```
