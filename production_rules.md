# JGoin's Rules For Production
## General
- **ASSUME ABSOLUTELY NOTHING**.  Your environment may not conform to anything even REMOTELY close to best practices.
- Don't be clever.  Be clear.
- File a **JIRA TICKET** or it **NEVER HAPPENED**.

## Documentation
- If it's going into production, it needs documentation (Confluence, MkDocs, ReadTheDocs, etc.)
  - Comments in code do not qualify as documentation.
  - A `README.md` in a code repository does not (fully) qualify as documentation.
    - Have others been trained on how to use this?
    - What are the edge-cases/"gotchas"?
    - ???
- If it's a production service that another team is going to be responsible for supporting, **YOU** are responsible for:
  - Writing documentation on how to recover said service(s)
  - Answering questions/being escalated to in the event documentation does not cover the edge-case.
  - Providing feedback and a post-mortem for ANY issue escalated to you.
- If it's a service outage/degredation:
  - A post-mortem + RCA must **ALWAYS** be performed.
  - Addressing the Root Causes discovered during Post-Mortem/Root Cause Analysis should be addressed on a schedule:
    - Immediate (~48 hours)
    - Short-term (~7-14 days)
    - Long-term (30+ days)
  - Root Causes should be escalated and visible to Management.
  - Root Causes should be discussed ***AT STAND-UP THE FOLLOWING BUSINESS DAY***.

## Logging
- If it's a daemon, it **SHOULD** log to ***A STANDARD LOGGING FACILITY SUCH AS SYSLOG***.
- If it's something that provides a response to customers, it should log access and responses **SOMEWHERE**.
- If it logs locally, **IT SHOULD BE ON A ROTATION SCHEDULE VIA LOGROTATE**.
- If it sends to a remote logging facility (ELK, Logz.io, Splunk, SumoLogic, etc.), local facilities should be on an **AGGRESSIVE** rotation schedule (~7 days)
- If it doesn't log anything to any location... **MAYBE IT SHOULD**.

# Monitoring
- If it's in production, **IT SHOULD BE MONITORED**.
- If it's a service or API, it should be actively monitored with:
  - synthetic transactions (canaries)
  - health-checks
  - HTTP response code checks
  - etc.
- Monitoring **SHOULD ALWAYS ALERT SOMEONE**.
- If it's not important, **DON'T ALERT ON IT**.  Instead:
  - Log it somewhere.
  - Send a stat somewhere.
  - Or figure out if it's worth paying attention to in the first place.

# Metrics (Stats)
- If it's a service, it should report metrics somewhere.
- If it's reporting data, it should be part of the operational procedures/runbooks/playbooks/etc.
- If it has a stats dashboard, it should be available on-demand and at-a-glance to teams impacted by said service(s).

# Testing/Tests
- Does the feature/cookbook/manifest/change/code have:
  - rspec
  - chefspec
  - serverspec
  - ???
- Does testing in a build environment (Kitchen, Jenkins, Travis, etc.) operate "as closely to Production as possible"?
- Do other services/servers/etc. interact with the feature/cookbook/manifest/change/code you are modifying?
- Do you provide a method to mock it to be tested against?
  - Is it published/available?
- Does your feature/cookbook/manifest/change/code interact with other services/servers/etc.?
- Does it have a mock API to talk to?
  - Is it resilient?
  - Does it recover gracefully?