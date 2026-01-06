GitHub Actions Summary for Certification Exam Preparation
This summary compiles key concepts, features, syntax, best practices, and components from the GitHub Actions documentation. It focuses on exam-relevant information, such as definitions, configurations, security, and troubleshooting. The content is organized by topic for easy reference.
1. Overview and Core Concepts

GitHub Actions is a CI/CD platform for automating build, test, deployment, and repository tasks.
Workflows: Custom automated processes defined in YAML files stored in .github/workflows. Triggered by events, run manually, or on schedules.
Events: Triggers like repository activities (e.g., push, pull_request), schedules, or manual (workflow_dispatch).
Jobs: Sets of steps executed on a single runner. Jobs run in parallel by default but can have dependencies.
Steps: Individual tasks in a job; can run scripts or use actions.
Actions: Reusable code units for common tasks (e.g., checkout repo, setup tools).
Runners: Servers hosting job execution; GitHub-hosted (Ubuntu, Windows, macOS) or self-hosted.
Common use cases: Building/testing on pull requests, deploying releases, labeling issues, scheduled backups.
Essential features: Event-driven, parallel/dependent jobs, matrix builds, reusable workflows/actions, self-hosted runners for custom environments.

2. Workflow Syntax and Structure

Workflow-level keys:
name: Display name (optional).
run-name: Dynamic run name (optional, uses expressions).
on: Required; events/triggers (e.g., push, pull_request, schedule, workflow_dispatch).
permissions: Controls GITHUB_TOKEN scopes (read/write/none; e.g., contents: read).
env: Environment variables for all jobs.
defaults: Default settings (e.g., shell).
concurrency: Limits runs (group, cancel-in-progress).
jobs: Required; map of job definitions.

Job-level keys:
id: Unique identifier (alphanumeric, -, _).
name: Display name.
needs: Dependencies (array of job IDs).
runs-on: Runner label (e.g., ubuntu-latest, self-hosted).
env, permissions, concurrency, timeout-minutes: Job-specific.
outputs: Values for downstream jobs.
strategy: For matrices (matrix, fail-fast, max-parallel).
continue-on-error: Proceed despite errors.
container: Run job in Docker container.
services: Additional containers (e.g., databases).
steps: Required; list of steps.

Step-level keys:
id: For outputs.
if: Conditional expression.
name: Display name.
uses: Action reference (e.g., actions/checkout@v5).
run: Inline script.
shell: Interpreter (bash, pwsh, etc.).
with: Action inputs.
env: Step variables.
continue-on-error, timeout-minutes: Error handling.

Reusable workflows: Use on: workflow_call; define inputs/outputs/secrets.
Best practices: Pin actions to SHA/tags, use least-privilege permissions, set timeouts, unique IDs, idempotent steps.

3. Events and Triggers

Categories:
Webhook: push, pull_request (types: opened, synchronize, etc.), issues, release, etc.
Scheduled: schedule with cron (e.g., '15 4 * * *').
Manual: workflow_dispatch (inputs), repository_dispatch, workflow_run.

Filters: branches/tags (e.g., main, v1.**), paths (e.g., **.js), types (e.g., opened, edited).
Defaults: pull_request triggers on opened, synchronize, reopened.
Notes: Events only trigger if workflow on default branch; restrictions in forks.

4. Contexts and Expressions

Contexts: Dynamic data objects.
github: Event/repo info (e.g., github.ref, github.sha, github.actor).
env: Environment variables.
vars: Config variables (org/repo/env levels).
job: Current job status/container.
jobs: Outputs in reusable workflows.
steps: Step outputs/conclusions.
runner: Runner details (os, arch, temp).
secrets: Secrets (e.g., GITHUB_TOKEN).
strategy: Matrix settings.
matrix: Current matrix values.
needs: Dependent job outputs/results.
inputs: For workflow_call/dispatch.

Expressions: ${{ }} syntax; literals (true, null, 5, 'string'), operators (==, &&, !), functions (contains, startsWith, format, join, toJSON, hashFiles, success, failure, always, cancelled).
Usage: In if, env, etc.; case-insensitive strings, loose equality.

5. Jobs and Matrices

Jobs run parallel; use needs for sequencing.
Matrix: strategy.matrix for variations (e.g., os: [ubuntu, windows], node: [14,16]).
include/exclude: Add/remove combinations.
max-parallel: Limit concurrency.
fail-fast: Cancel all on failure (default true).
Access: ${{ matrix.os }}.
Best practices: For cross-config testing; dynamic matrices via outputs.

6. Variables

Default: e.g., RUNNER_OS.
Custom:
env: Workflow/job/step levels.
vars: Org/repo/env levels (reusable across workflows).

Naming: Alphanumeric + _, case-sensitive.
Access: ${{ env.NAME }} or shell $NAME; vars via ${{ vars.NAME }}.
Precedence: Step > job > workflow.

7. Secrets

Levels: Org (with access policies), repo, env.
Access: ${{ secrets.NAME }}; not in if directly (use env intermediate).
Best practices: Mask (::add-mask::), avoid CLI, use OIDC, rotate on exposure.
Limitations: 48KB, not in forks/reusable without explicit pass, encrypted at rest.

8. Artifacts

Upload: actions/upload-artifact@v4 (path, name, retention-days).
Download: actions/download-artifact@v5 (name or all).
Retention: Custom or repo/org default.
Sharing: Use needs for inter-job; digest for validation.
Use cases: Logs, builds; immutable in v4.

9. Caching

Action: actions/cache@v4 (key, path, restore-keys).
Key: Unique (e.g., ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}).
Restore-keys: Fallbacks (specific to general).
Hit/miss: Exact key hit restores; miss uses restore, creates new on success.
Limitations: 10GB/repo, 7-day eviction, no modification.
Best practices: hashFiles for deps, avoid secrets, use setup-* actions for auto-cache.

10. Containers and Services

Job container: container: image (Linux only).
Services: Map of containers (e.g., redis: image: redis, ports).
Requirements: Docker on runner; volume mapping.

11. Security Best Practices

Permissions: Least privilege for GITHUB_TOKEN.
Actions: Pin to SHA, audit code.
Injections: Intermediate env for untrusted input.
Reviews: CODEOWNERS for workflows.
Dependencies: Dependabot alerts/updates, Scorecards.
Secrets: Mask, rotate, review access.

12. Monitoring and Troubleshooting

View: History, logs, re-run failed jobs.
Debug: Step-by-step, secrets in logs (redacted), notifications.
Common: Permission errors, timeouts, syntax issues.

13. Limits, Billing, Administration

Limits: Minutes/storage quotas, concurrent jobs.
Billing: Free public/self-hosted; private quotas by plan (multipliers for larger runners).
Admin: Disable/enable workflows, set policies, view metrics.
Reusable: Caller pays; no runner inheritance.

14. Reusable Workflows

Define: on: workflow_call, inputs (type, required, default), outputs, secrets.
Call: uses: path@ref, with inputs, secrets (or inherit).
Limitations: 10-level nesting, no loops, permissions reduced.

200 Exam-Style Questions
These questions cover the summary above. Types include multiple-choice (MCQ), true/false (T/F), and short answer (SA). Answers are provided at the end of each question for self-study.

MCQ: What is the primary purpose of GitHub Actions?
a) Version control
b) CI/CD automation
c) Issue tracking
d) Wiki management
Answer: b
T/F: Workflows are stored in the .github/workflows directory.
Answer: True
MCQ: Which key is required in a workflow YAML file?
a) name
b) on
c) permissions
d) env
Answer: b
SA: Name two types of runners in GitHub Actions.
Answer: GitHub-hosted, self-hosted
MCQ: What does the needs key in a job specify?
a) Runner type
b) Dependencies on other jobs
c) Environment variables
d) Action inputs
Answer: b
T/F: Jobs in a workflow run sequentially by default.
Answer: False (parallel)
MCQ: Which context provides event information like ref and sha?
a) env
b) github
c) runner
d) matrix
Answer: b
MCQ: What is the syntax for expressions in workflows?
a) {{ }}
b) ${{ }}
c) <% %>
d) [[ ]]
Answer: b
T/F: String comparisons in expressions are case-sensitive.
Answer: False
MCQ: Which function checks if a string starts with a value?
a) contains
b) startsWith
c) format
d) hashFiles
Answer: b
SA: What status function always evaluates to true?
Answer: always()
MCQ: Where are custom configuration variables defined for reuse across workflows?
a) env context
b) vars context
c) secrets context
d) github context
Answer: b
T/F: Variables in env take precedence over vars.
Answer: True (env is workflow-specific)
MCQ: At which level can secrets NOT be created?
a) Organization
b) Repository
c) Environment
d) Step
Answer: d
MCQ: How are secrets accessed in workflows?
a) ${{ env.SECRET }}
b) ${{ vars.SECRET }}
c) ${{ secrets.SECRET }}
d) ${{ github.SECRET }}
Answer: c
T/F: Secrets can be used directly in if conditions.
Answer: False (use intermediate env)
MCQ: Which action is used to upload artifacts?
a) actions/cache
b) actions/upload-artifact
c) actions/download-artifact
d) actions/setup-node
Answer: b
SA: What input sets a custom retention period for artifacts?
Answer: retention-days
T/F: Artifacts can be shared between jobs without needs.
Answer: False
MCQ: What is the default cache size limit per repository?
a) 1GB
b) 5GB
c) 10GB
d) Unlimited
Answer: c
MCQ: Which parameter in actions/cache provides fallback keys?
a) key
b) path
c) restore-keys
d) enableCrossOsArchive
Answer: c
T/F: Caches can be modified after creation.
Answer: False
MCQ: What function generates a hash for cache keys?
a) toJSON
b) hashFiles
c) format
d) join
Answer: b
SA: How long do unused caches last before eviction?
Answer: 7 days
MCQ: In a matrix strategy, how do you access a variable like 'os'?
a) ${{ strategy.os }}
b) ${{ matrix.os }}
c) ${{ job.os }}
d) ${{ env.os }}
Answer: b
T/F: The include key in matrix adds combinations.
Answer: True
MCQ: What limits concurrent matrix jobs?
a) fail-fast
b) max-parallel
c) continue-on-error
d) needs
Answer: b
MCQ: Which key defines a job container?
a) runs-on
b) container
c) services
d) strategy
Answer: b
T/F: Service containers can expose ports.
Answer: True
SA: Name a requirement for using containers in jobs.
Answer: Docker on runner
MCQ: What is a best practice for third-party actions?
a) Use main branch
b) Pin to SHA
c) Avoid auditing
d) Grant write-all
Answer: b
T/F: Use least-privilege for GITHUB_TOKEN.
Answer: True
MCQ: How to mask sensitive log output?
a) ::mask::
b) ::add-mask::
c) ::redact::
d) ::hide::
Answer: b
MCQ: Which tool helps with dependency vulnerabilities?
a) CODEOWNERS
b) Dependabot
c) Scorecards
d) All of the above
Answer: d
SA: What file requires review with CODEOWNERS?
Answer: Workflow files
MCQ: Where can you view workflow logs?
a) Issues tab
b) Actions tab
c) Settings
d) Pull requests
Answer: b
T/F: Workflows can be re-run.
Answer: True
MCQ: What is free for public repositories?
a) Actions usage
b) Private runners
c) Unlimited storage
d) All plans
Answer: a
SA: Who pays for reusable workflow billing?
Answer: Caller workflow
MCQ: Max nesting for reusable workflows?
a) 5
b) 10
c) 20
d) Unlimited
Answer: b
T/F: Reusable workflows use on: workflow_call.
Answer: True
MCQ: How to pass all secrets to reusable?
a) secrets: all
b) secrets: inherit
c) secrets: pass
d) secrets: true
Answer: b
MCQ: Which event is NOT a webhook?
a) push
b) schedule
c) pull_request
d) issues
Answer: b
T/F: Path filters use AND with branches.
Answer: True
SA: Default types for pull_request event?
Answer: opened, synchronize, reopened
MCQ: Which context is for runner OS?
a) github
b) runner
c) job
d) strategy
Answer: b
MCQ: Function to convert to JSON?
a) format
b) toJSON
c) fromJSON
d) join
Answer: b
T/F: failure() returns true if any prior step failed.
Answer: True
MCQ: Variable naming allows what characters?
a) Spaces
b) Underscores
c) Dashes
d) Periods
Answer: b
SA: Precedence order for env variables?
Answer: Step > job > workflow
MCQ: Secrets size limit?
a) 1KB
b) 48KB
c) 1MB
d) Unlimited
Answer: b
T/F: Secrets are passed to forks.
Answer: False
MCQ: Artifact download action version?
a) v1
b) v3
c) v5
d) v4
Answer: c
SA: What validates artifact integrity?
Answer: Digest (SHA256)
T/F: Artifacts are mutable in v4.
Answer: False
MCQ: Cache action output for hit?
a) cache-hit
b) cache-miss
c) hit
d) match
Answer: a
MCQ: Cache eviction after inactivity?
a) 1 day
b) 7 days
c) 30 days
d) Never
Answer: b
T/F: Cache sensitive data.
Answer: False
MCQ: Matrix exclude removes what?
a) All jobs
b) Specific combinations
c) Variables
d) Steps
Answer: b
SA: Context for matrix job index?
Answer: strategy.job-index
MCQ: Container jobs limited to?
a) Windows
b) Linux
c) macOS
d) All
Answer: b
T/F: Services are reachable at localhost.
Answer: True
MCQ: Avoid caching what?
a) Dependencies
b) Tokens
c) Builds
d) Logs
Answer: b
MCQ: For script injections, use?
a) Direct input
b) Intermediate env
c) Echo
d) Print
Answer: b
SA: Tool for action vulnerabilities?
Answer: Dependabot
T/F: Debug with printed secrets.
Answer: False (redacted)
MCQ: Billing multipliers for?
a) Small runners
b) Larger runners
c) Public repos
d) Self-hosted
Answer: b
SA: Max concurrent jobs limit based on?
Answer: Plan
MCQ: Reusable workflows inherit?
a) Runners
b) Permissions (reduced)
c) Events
d) All secrets automatically
Answer: b
T/F: Loops allowed in workflow calls.
Answer: False
MCQ: Cron for scheduled events?
a) Every hour
b) '0 * * * *'
c) Both
d) None
Answer: c
MCQ: github.token is?
a) Secret
b) Variable
c) Context property
d) Action
Answer: c
T/F: Expressions use strict equality.
Answer: False (loose)
SA: Function for array join?
Answer: join
MCQ: Vars defined in settings for?
a) Org/repo/env
b) Workflow only
c) Steps only
d) Jobs only
Answer: a
T/F: Secrets encrypted at rest.
Answer: True
MCQ: Artifact default name?
a) artifact
b) build
c) log
d) cache
Answer: a
SA: Cache cross-OS enable param?
Answer: enableCrossOsArchive
T/F: Matrix fail-fast default true.
Answer: True
MCQ: Container options include?
a) image
b) ports
c) volumes
d) All
Answer: d (for services; container has image/options)
MCQ: Pin actions to avoid?
a) Updates
b) Backdoors
c) Speed
d) Costs
Answer: b
T/F: Use double quotes for shell vars.
Answer: True (avoid splitting)
SA: Audit tool for actions?
Answer: OpenSSF Scorecards
MCQ: Re-run workflows from?
a) UI
b) API
c) CLI
d) All
Answer: d
T/F: Private repos have free quotas.
Answer: True
MCQ: Reusable inputs type?
a) string only
b) boolean/number/string
c) array
d) object
Answer: b
SA: Trigger for reusable?
Answer: workflow_call
MCQ: Path filters negate with?
a) -
b) !
c) ~
d) *
Answer: b
T/F: Contexts available in if.
Answer: True
MCQ: fromJSON parses?
a) String to JSON
b) JSON to string
c) Hash
d) Array only
Answer: a
MCQ: Org variables access?
a) All repos
b) Selected
c) Both
d) None
Answer: c
T/F: Secrets in composite actions.
Answer: False (not available)
SA: Artifact mismatch triggers?
Answer: Warning
MCQ: Cache search order?
a) Current branch then default
b) Default then current
c) Random
d) Alphabetical
Answer: a
T/F: Include overrides matrix.
Answer: True
MCQ: Services for?
a) Databases
b) Runners
c) Variables
d) Secrets
Answer: a
MCQ: Rotate secrets on?
a) Exposure
b) Weekly
c) Never
d) Install
Answer: a
T/F: Metrics view requires permission.
Answer: True
MCQ: Nested workflows permissions?
a) Elevated
b) Reduced/maintained
c) Unlimited
d) None
Answer: b
SA: Max cache per job?
Answer: Not specified, but total 10GB/repo
MCQ: Which is a manual event?
a) push
b) workflow_dispatch
c) schedule
d) release
Answer: b
T/F: Expressions can omit ${{ }} in if.
Answer: True (sometimes)
MCQ: needs context for?
a) Outputs
b) Variables
c) Secrets
d) Events
Answer: a
SA: Secret workaround for >48KB?
Answer: Encrypt and store passphrase
T/F: Download all artifacts creates directories.
Answer: True
MCQ: Cache partial match uses?
a) Oldest
b) Most recent
c) Random
d) Smallest
Answer: b
MCQ: Matrix job-total in?
a) matrix
b) strategy
c) job
d) steps
Answer: b
T/F: Containers require Linux runners.
Answer: True
MCQ: Dependency review in?
a) Issues
b) Pull requests
c) Actions
d) Settings
Answer: b
SA: Disable Actions via?
Answer: Settings/policies
MCQ: Outputs in reusable from?
a) Steps
b) Jobs
c) Workflow
d) All
Answer: b
T/F: Environment secrets passed to reusable.
Answer: False
MCQ: Cron minimum interval?
a) 1 min
b) 5 min
c) 10 min
d) 60 min
Answer: b
MCQ: github.event_name for?
a) Actor
b) Event type
c) Ref
d) SHA
Answer: b
T/F: NaN comparisons true.
Answer: False
SA: Escape { } in format?
Answer: {{ }}
MCQ: Vars return on unset?
a) Error
b) Null
c) Empty string
d) False
Answer: c
T/F: Binary secrets as Base64.
Answer: True
MCQ: Artifact immutable in?
a) v1
b) v2
c) v4
d) All
Answer: c
SA: Cache no match returns?
Answer: Empty string (hashFiles)
T/F: exclude partial match.
Answer: True
MCQ: Services ports dynamic in?
a) job.services
b) runner.temp
c) github.port
d) env.port
Answer: a
MCQ: Avoid structured secrets why?
a) Size
b) Redaction
c) Speed
d) Cost
Answer: b
T/F: Usage metrics include multipliers.
Answer: False
MCQ: Reusable max levels?
a) 4
b) 10
c) 16
d) 100
Answer: b
SA: Call reusable with?
Answer: uses
MCQ: Issue event types include?
a) opened
b) edited
c) Both
d) None
Answer: c
T/F: Contexts in composite actions.
Answer: True (limited)
MCQ: cancelled() for?
a) Success
b) Failure
c) Cancelled workflow
d) Always
Answer: c
MCQ: Org vars not in?
a) Free private repos
b) Public
c) Enterprise
d) All
Answer: a
T/F: OIDC alternative to secrets.
Answer: True
SA: Upload artifact exclude with?
Answer: !pattern
MCQ: Cache on miss?
a) Creates new
b) Errors
c) Skips
d) Downloads
Answer: a
T/F: Multi-dimensional matrices supported.
Answer: True
MCQ: Volume mapping in?
a) Containers
b) Variables
c) Secrets
d) Artifacts
Answer: a
MCQ: Scorecards flag?
a) Risks
b) Costs
c) Speed
d) Size
Answer: a
T/F: Re-run includes failed only.
Answer: True (option)
MCQ: Billing for self-hosted?
a) Minutes
b) Free
c) Storage
d) Concurrent
Answer: b
SA: Secrets in reusable?
Answer: Explicit or inherit
MCQ: Discussion event?
a) Webhook
b) Scheduled
c) Manual
d) None
Answer: a
T/F: Index syntax for contexts.
Answer: True (github['sha'])
MCQ: endsWith case?
a) Sensitive
b) Insensitive
c) Both
d) None
Answer: b
MCQ: Variable creation requires?
a) Admin
b) Write
c) Read
d) Owner
Answer: b (repo)
T/F: Secrets audit logs.
Answer: True (usage)
SA: Artifact retention max?
Answer: Repo/org limit
MCQ: Restore-keys order?
a) General to specific
b) Specific to general
c) Random
d) Alphabetical
Answer: b
T/F: continue-on-error per matrix.
Answer: True
MCQ: Services image from?
a) Docker Hub
b) GHCR
c) Both
d) None
Answer: c
MCQ: Least privilege for?
a) Tokens
b) Runners
c) Events
d) All
Answer: a
SA: Monitor usage in?
Answer: Settings/metrics
T/F: Workflow tree no loops.
Answer: True
MCQ: Merge_group event for?
a) Queues
b) Pushes
c) Issues
d) Releases
Answer: a
MCQ: Property syntax invalid for?
a) Numbers start
b) Letters
c) Underscores
d) Dashes
Answer: a
T/F: Objects equal if same instance.
Answer: True
SA: Env vars in run as?
Answer: $VAR
MCQ: Secrets required in?
Answer: workflow_call.secrets
T/F: Digest recalculated on download.
Answer: True
MCQ: Cache eviction oldest?
a) First
b) Last
c) Random
d) Largest
Answer: a
MCQ: Matrix outputs order?
Answer: Not guaranteed
T/F: Docker for self-hosted.
Answer: True
SA: Pin to tag if?
Answer: Trusted creator
MCQ: Dependabot for?
a) Updates
b) Alerts
c) Both
d) None
Answer: c
T/F: Notifications for errors.
Answer: True
MCQ: Admin can?
a) Disable Actions
b) View metrics
c) Both
d) None
Answer: c
SA: Outputs value from?
Answer: jobs.<id>.outputs
MCQ: Watch event types?
a) started
b) stopped
c) Both
d) None
Answer: a
T/F: ! in if escaped.
Answer: True
MCQ: hashFiles globs?
a) Yes
b) No
c) Partial
d) Only files
Answer: a
MCQ: Env override order?
a) Workflow > job > step
b) Step > job > workflow
c) Job > step > workflow
d) Random
Answer: b
T/F: PAT instead of GITHUB_TOKEN.
Answer: True (for more scopes)
SA: Artifact path multiple?
Answer: Yes, lines
MCQ: Cache max points?
a) 20-40
b) Unlimited
c) 10
d) 100
Answer: a (general tip, not specific)
T/F: Exclude overwrites include.
Answer: False
MCQ: Services health check?
a) Options
b) Image
c) Ports
d) All
Answer: a
MCQ: Structured data as secrets?
a) Recommended
b) Avoid
c) Required
d) Optional
Answer: b
T/F: Enterprise policies for Actions.
Answer: True
MCQ: Reusable subdirs?
a) Supported
b) Not
c) Partial
d) Only local
Answer: b
SA: Pull_request_target for?
Answer: Safer for untrusted code
MCQ: Literals quotes in ${{ }}?
a) Double
b) Single
c) None
d) Both
Answer: b
T/F: Arrays equal if same.
Answer: True (instance)
MCQ: Org secrets policy?
a) All
b) Selected
c) Private
d) All above
Answer: d
SA: Binary decode with?
Answer: base64 --decode
T/F: Upload returns digest.
Answer: True
MCQ: Cache branch scope?
a) All
b) Current/default/PR
c) None
d) Forks
Answer: b
MCQ: Matrix experimental with?
a) continue-on-error
b) fail-fast
c) max-parallel
d) include
Answer: a
T/F: Entry point override in Docker.
Answer: True
MCQ: Code scanning for?
a) Workflows
b) Vulnerabilities
c) Both
d) None
Answer: c
SA: Cancel-in-progress in?
Answer: concurrency
MCQ: Retention for logs?
a) Configurable
b) Fixed
c) Unlimited
d) None
Answer: a
T/F: Inputs in workflow_dispatch.
Answer: True
MCQ: Steps conclusion vs outcome?
a) Same
b) Outcome before continue-on-error
c) Conclusion before
d) None
Answer: b
SA: GITHUB_ENV for?
Answer: Set env between steps
T/F: Secrets in logs redacted.
Answer: True
MCQ: Cache setup actions?
a) Auto
b) Manual
c) Both
d) None
Answer: a
MCQ: Exclude match?
a) Full
b) Partial
c) Both
d) None
Answer: b
T/F: Services volume map.
Answer: True
MCQ: Periodically audit?
a) Secrets
b) Variables
c) Both
d) None
Answer: a
SA: Metrics permission?
Answer: View organization Actions metrics
MCQ: Workflow_run types?
a) completed
b) requested
c) Both
d) None
Answer: c
T/F: Reusable cannot elevate permissions.
Answer: True
