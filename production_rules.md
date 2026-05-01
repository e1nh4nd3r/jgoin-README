# JGoin's Rules For Production
## General
- **ASSUME ABSOLUTELY NOTHING**.  Your environment may not conform to anything even REMOTELY close to "best practices".
- Don't be clever, be clear.
- Submit an issue to the System of Record (SOR).
  - "Show me the JIRA Issue or it NEVER HAPPENED!"

## Documentation
- If it's going into production, it needs documentation (Confluence, MkDocs, ReadTheDocs, etc.)
  - Comments in code do not qualify as documentation.
  - A `README.md` in a code repository does not (fully) qualify as documentation.
    - Have others been trained on how to use this?
    - What is the primary use-case and "golden path" for using this?
    - What are the edge-cases/"gotchas"?
    - ???
- If it's a production service that another team is going to be responsible for supporting, **YOU** are responsible for:
  - Writing documentation on how to recover said service(s) in the event of a loss of HA, performance degradation, a DR event, etc.
  - Answering questions/being escalated to in the event documentation does not cover something.
  - Providing feedback and a post-mortem for ANY issue escalated to you.
- If it's a service outage/degredation:
  - A post-mortem + RCA must **ALWAYS** be performed, even if just within the team or group supporting the service.
  - Addressing the Root Causes discovered during Post-Mortem/Root Cause Analysis should be addressed on a schedule:
    - Immediate (~48 hours)
    - Short-term (~7-14 days)
    - Long-term (30+ days)
  - Root Causes should be escalated and visible to Management.
  - Root Causes should be discussed ***AT STAND-UP THE FOLLOWING BUSINESS DAY***.

## Logging
- If it's a daemon, service, binary/application, cron job, ANYTHING--it **SHOULD** log to ***A STANDARD LOGGING FACILITY SUCH AS SYSLOG***.
- If it's something that provides a response to a human or another service/system accessing it, it should log access and responses **SOMEWHERE**.
- If it logs locally, **IT SHOULD BE ON A ROTATION SCHEDULE**.
- If it sends to a remote logging facility (`ELK`, `Logz.io`, `Splunk`, `SumoLogic`, etc.), local facilities should be on an **AGGRESSIVE** rotation schedule (~7 days)
- If it doesn't log anything to any location... **MAYBE IT SHOULD**.

## Monitoring
- If it's in Production, **IT SHOULD BE MONITORED**.
  - Even if it's not in Production, maybe it should be monitored.
- If it's a service or API, it should be actively monitored with:
  - synthetic transactions (canaries)
  - health-checks
  - HTTP response code checks
  - etc.
- Monitoring **SHOULD ALWAYS ALERT SOMEONE**.
- If it's not important, **DON'T ALERT ON IT**.  Instead:
  - Log it somewhere.
  - Send a metric somewhere.
  - Figure out if it's worth paying attention to in the first place.

## Metrics
- If it's a service, it should record metrics somewhere.
- If it's reporting data, it should be part of the operational procedures/runbooks/playbooks/etc.
- If it has a dashboard, it should be available on-demand and at-a-glance to teams impacted by or consuming the service(s).

## Testing/Tests
- Does the feature/cookbook/manifest/change/code have:
  - Testing?
    - Spec tests
    - Integration tests
    - Linting/Style tests
- Does testing in a local environment (Chef-Kitchen, Jenkins, Travis, etc.) produce results "as close to Production as possible"?
- Do other services/servers/etc. interacting with the feature/cookbook/manifest/change/code you are modifying?
- Does it provide a method to mock it to be tested against?
  - Is it published/available?
- Does your feature/cookbook/manifest/change/code interact with other services/servers/etc.?
- Does it have a mock API to talk to?
  - Is it resilient?
  - Does it recover gracefully?