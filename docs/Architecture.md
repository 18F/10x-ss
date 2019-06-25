# Architecture

There are three components to this project:
- *The Scanning Engine:*  This engine, when executed, will run all the configured scans 
  against all of the configured domains and generate json output.
- *Scanning Engine Plugins:*  These will be plugins that will let you execute different
  types of scans.  They will be used by the scanning engine, or can be executed by hand.
  It is anticipated that over time, more plugins will be created to expand the functionality
  of the scanning engine.
- *Scan Result UI:*  This is a web frontend that implements a REST API to download/search
  scan results.

  The scan API map is thus:
  - `/api/v1/domains/` lists the domains and what scan types are available for each of them.
  - `/api/v1/domains/{domain}` pulls down all of the scan results for a particular domain.
  - `/api/v1/domains/{domain}/scan/{scantype}` pulls down the particular scan result for a particular domain.
  - `/api/v1/scans/` lists the scan types and what domains have scan data available for them.
  - `/api/v1/scans/{scantype}` pulls down all of the scan results for all domains that have this scantype.
  - `/api/v1/scans/{scantype}/domain/{domain}` pulls down the particular scan result for a particular domain.

The components are deployed using CircleCI into cloud.gov and once a day, the scanning engine is
executed using https://docs.cloudfoundry.org/devguide/using-tasks.html#run-tasks, generating
it's json files into S3.  The UI then can be used to find and download the scan results.