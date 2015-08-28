# Fish Hook’s High-Level Requirements

## Terminology
- **Code Signing**: Putting a “seal” on compiled source code (e.g. an app) that verifies the package was created by a source Apple has verified as authentic
- **Provisioning Profile**: A document that is embedded within an app that determines 1. the originating Apple developer account 2. white-lists the specific iOS devices the app is allowed to run on 3. indicates privileges to Apple services (iCloud, push notifications, etc) or system behaviors (Keychain, etc)
- **Enterprise Build**: Compiling iOS source code and code signing with a distribution certificate tied to an Enterprise account. Will run on any iOS device, but app will “phone home” to Apple to make sure Enterprise account is in good standing.
- **Ad Hoc Build**: Compiling iOS source code and code signing with a distribution certificate tied to an App Store account. Build will only run on iOS devices that are known at the time the build was created.
- **App Store Build**: Compiling iOS source code and code signing with a distribution certificate tied to an App Store account. Build can only be submitted to Apple for app review.

## Before 2015
- Client must have two developer accounts. $99 iOS Developer Account and $299 Enterprise Apple Developer Account. Each account requires a EIN and DUNS; client should sign up a subsidiary / holding company as the Enterprise account holder. Takes about two weeks for Apple to green-light an account.
- Distribute an enterprise build via HockeyApp.net for nightly builds to stakeholders
- Distribute an enterprise build via HockeyApp.net for internal “bug bash” pre-release events
- Client must funnel bug reports / feature requests / etc into centralized spot: JIRA, GitHub, custom CMS, etc. Cannot not be email, Slack, iMessage.

## After 2015
- Client must have one developer account: $99 Apple Developer Account. Account requires EIN and DUNS; takes two weeks, often times faster.
- Distribute Ad Hoc build via HockeyApp.net for nightly builds to stakeholders
- Distribute App Store build via TestFlight for internal bug bash pre-release events
- Centralized bug tracker

## Workflow
1. Developers work Mon - Fri on bug fixes, features, and technical debt backlog.
2. Build server kicks off a build Thursday night at 3am Eastern. Uploads new build to Hockey and emails stakeholders the list of commits scraped from git.
3. Stakeholders come in Friday morning, download new build, have standup meeting to clarify what’s new / discuss blockers / set up goals for next week. Stakeholders go off and QA “nightly” build.
4. Rinse-and-repeat for one to twelve weeks long cycle
5. After cycle, recruit larger internal team for bug bash. Should be good cross-section of organization (design, customer service, leadership, etc)
6. Distribute a pretty-much-finished enterprise build to bug bash team
7. Get pizza & beer and bring internal team in for two to four hour bug bash. Begin bash with a marketing pitch about the upcoming release; feedback on marketing pitch is useful. No bug, annoyance, unexpected behavior, or feature request is too small.
8. Project Manager collects and logs all stories into bug tracker.
9. Stakeholders decide what are show-stoppers, prioritizes bugs for one to four week final push
10. Rinse-and-repeat for the remainder of year
