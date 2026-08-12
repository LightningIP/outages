# Public Outage \& Maintenance Register

This repository contains publicly accessible outage and planned maintenance information published as part of our commitment to transparency, customer communication, and regulatory compliance.

It provides a searchable historical record of outages and maintenance activities, including affected locations, impacted service types, timing information, causes where appropriate, and public update history.

## Purpose

This repository acts as a public register and audit trail for organisation-owned outage and maintenance records.

Information is published in machine-readable JSON and may be:

* Viewed through the associated public website.
* Accessed directly from the repository.
* Used to review current and planned events.
* Browsed historically by calendar month.
* Consumed by third-party applications and automated systems.

The objectives of this repository are to:

* Improve transparency around outages and maintenance activities.
* Provide customers with accessible historical outage information.
* Support applicable reporting and evidence-retention requirements.
* Maintain an auditable history of published outage updates.
* Enable public access to outage information using open, machine-readable formats.

## Repository Structure

```text
/
├── index.html
├── current.json
├── planned.json
│
├── summary/
│   └── YYYY/
│       └── MM.json
│
└── outages/
    └── YYYY/
        └── MM/
            └── OUTAGE-ID.json
```

## Website

`index.html` provides the public user interface for browsing outage and maintenance information.

The website may use the JSON files in this repository to provide features such as:

* Viewing current outages.
* Viewing planned maintenance.
* Browsing records by month.
* Searching by outage reference.
* Filtering by state, suburb, postcode, or service type.
* Viewing the complete update history for an outage.

The JSON files remain directly accessible independently of the website.

## Data Files

### Current Outages

`current.json` contains summary records for outages that are currently active.

The file is intended to provide a lightweight entry point for the public website and other consumers without requiring every full outage record to be downloaded.

### Planned Maintenance

`planned.json` contains summary records for future scheduled maintenance activities.

An event may move from planned maintenance to current activity when its scheduled work window begins.

### Monthly Summaries

Monthly summary files are stored at:

```text
summary/YYYY/MM.json
```

Each file contains summary records for outages and maintenance activities relevant to that calendar month.

An event may appear in multiple monthly summary files. This may occur where the event:

* Was created or first published during the month.
* Was scheduled to occur during the month.
* Remained relevant to customers during the month.
* Was active during the month.
* Received a public update during the month.
* Was resolved, completed, cancelled, or rectified during the month.

For example, maintenance announced in July, scheduled for December, and completed in January may be discoverable through each relevant monthly summary from July through January.

Monthly summary files are indexes. They do not replace the full outage record.

Each summary entry should identify the creation year and month, or provide a direct record path, so that consumers can locate the authoritative full outage JSON file.

### Full Outage Records

Full outage records are stored at:

```text
outages/YYYY/MM/OUTAGE-ID.json
```

The year and month in the path represent the creation date used for the permanent storage location of the outage record.

A full record may contain:

* Unique outage reference.
* Outage title.
* Event type.
* Planned or unplanned classification.
* Category.
* Current status.
* Severity, where used.
* Timing and lifecycle information.
* Customer and operational impact indicators.
* Geographic impact.
* Affected service types and counts.
* Cause information.
* Current public summary.
* Customer guidance.
* Public update history.
* Publication metadata.

The full outage record is the authoritative public record for that outage within this repository.

## Date and Time Format

Dates and times should be published using ISO 8601 timestamps and will include timezone information where applicable, if no timezone is determined, then Australia/Sydney is used.

Example:

```text
2026-08-09T14:00:00.000Z
```

Timestamps should include an explicit timezone or UTC offset.

The public website may display timestamps in a local timezone, but the underlying JSON should retain an unambiguous timestamp.

## Missing or Unknown Information

Where information is not known, the publisher may:

* Use `null` for a field whose value is currently unknown.
* Use an empty array where no values are available.
* Omit an optional field where it does not apply.

Unknown values should not be represented as zero unless zero is the confirmed value.

For example:

```json
{
  "severity": null,
  "postcodes": \[],
  "actual\_restoration\_at": null
}
```

## Service Counts

`service\_count` represents the number of distinct affected service records known to the publication process.

Where available, the count may also be broken down by service type:

```json
{
  "services": {
    "service\_count": 181,
    "service\_types": \[
      "NBN - FTTN",
      "NBN - FTTP"
    ],
    "services\_by\_type": {
      "NBN - FTTN": 124,
      "NBN - FTTP": 57
    }
  }
}
```

Customer and premises counts may be omitted where they are not known or cannot be reliably derived from the available service information.

Individual service identifiers are not intended to be included in the public files.

## Geographic Information

Geographic information may include:

* States and territories.
* Suburbs or towns.
* Postcodes.
* Regions.
* Points of interconnect.
* Exchanges.
* Points of presence.
* Data centres.

Only geographic information suitable for public release should be included.

Where geography is derived from affected service records, duplicate values should be removed before publication.

## Update History

Each outage should preserve its public update history as an ordered array.

Updates should not be overwritten merely because a newer update has been received.

A public update may contain:

```json
{
  "update\_id": "8e945553-5fa5-4651-b510-6282ce5d211c",
  "update\_time": "2026-07-27T03:52:03.261Z",
  "update\_type": "initial\_update",
  "content": "We are investigating an interruption affecting services in the listed areas.",
  "material\_change": true
}
```

Updates should be ordered consistently, preferably from oldest to newest.

Corrections may be published as additional updates so that the public history remains understandable.

## Design Principles

This repository follows these principles:

* Public transparency wherever possible.
* Machine-readable publication.
* Clear, stable, and predictable file paths.
* Preservation of public update history.
* Deterministic generation of summary files.
* Separation of public outage information from private operational data.
* Protection of customer and service-specific information.
* Reproducible and auditable publication changes.
* Accessibility through both a website and direct JSON access.

## Data Sources

Outage information may originate from multiple carrier, wholesale, infrastructure, and technology partners.

Vendor information is normalised into an organisation-owned outage record before publication.

The public record may combine information from more than one vendor event where those events relate to the same outage or maintenance activity.

Raw vendor notifications, vendor API payloads, and vendor-specific internal records are not intended to be published in this repository.

## Public and Internal Data Boundary

This repository contains public, organisation-owned outage records.

It is not intended to contain:

* Raw vendor payloads.
* Internal correlation logic.
* Private vendor communications.
* Customer account information.
* Individual service identifiers.
* Internal-only investigation notes.
* Credentials or configuration secrets.
* Private customer communication records.
* Security-sensitive network information.

The internal outage-management system remains responsible for vendor ingestion, correlation, workflow state, and operational processing.

## Data Format

JSON is the primary publication format.

JSON supports:

* Direct browser access.
* Machine-readable consumption.
* Client-side searching and filtering.
* Efficient monthly indexes.
* Schema validation.
* Long-term archival.
* Review through Git history.

No authentication should be required to access files intended for public publication.

## Important Notes

* Published information reflects the best information available at the time of publication.
* Outage and maintenance details may change as investigations or work progress.
* Historical records may receive corrections or additional updates.
* Service counts and affected locations may change as more information becomes available.
* Causes may change as investigations progress.
* Maintenance windows may be rescheduled, shortened, extended, or cancelled.
* Estimated restoration times are estimates and may change.
* A missing or `null` value means that the information is not known, does not apply, or has not been approved for public release.
* Git history may be used to review how published files changed over time.

## Data Accuracy and Corrections

Outage and maintenance information may change as investigations progress, work is rescheduled, affected services are identified, or restoration activities are completed.

Where an error is identified in a published record:

1. The affected full outage record should be corrected.
2. The record's `updated\_at` timestamp should be changed.
3. A correction or clarification may be added to the public update history.
4. Relevant current, planned, and monthly summary files should be regenerated.
5. The correction should be committed with a clear commit message.
6. Git history should preserve evidence of the published change.

Corrections should improve the accuracy of the record without deleting legitimate historical updates.

## Privacy and Security

This is a public repository. Every committed file and its Git history must be treated as publicly accessible.

The following information must not be published:

* Customer names.
* Customer contact details.
* Customer account identifiers.
* Individual service identifiers.
* Authentication credentials.
* API keys or tokens.
* Private certificates or keys.
* Raw vendor payloads containing non-public information.
* Internal-only investigation notes.
* Security-sensitive technical information.
* Information that could create an unreasonable privacy, security, or operational risk.

Removing sensitive information in a later commit may not remove it from Git history. Sensitive information must be excluded before the original commit is created.

## Automated Publication

Files in this repository may be generated and updated by automated workflows.

Automated publication should:

* Produce deterministic JSON.
* Validate JSON before committing.
* Use the defined schema version.
* Avoid duplicate outage records.
* Preserve public update history.
* Regenerate every monthly summary relevant to a changed outage.
* Update `current.json` and `planned.json` where applicable.
* Exclude customer-specific, private, and security-sensitive information.
* Use clear commit messages containing the public outage reference.
* Avoid committing when no public data has changed.

Generated JSON files should not be edited manually unless the same change is also made in the authoritative source system or publication workflow.

Otherwise, a later automated publication may overwrite the manual change.

## Issues and Corrections

If you identify information that appears to be incorrect, incomplete, or inaccessible, please raise an issue in this repository or contact the organisation through its official support channels.



When reporting a possible data issue, include:

* The outage reference.
* The affected file path.
* A description of the issue.
* The information believed to be incorrect.
* Any publicly verifiable supporting information.



Do not include customer details, service identifiers, account information, credentials, or other sensitive information in a public issue.



Repository issues are intended for corrections to published information and technical problems with the public data. They must not be used for urgent service restoration requests or customer-specific support.



ContriContributions

This repository does not accept external contributions.



## Disclaimer

The information in this repository is provided for transparency and general informational purposes.

Outage details, affected areas, affected service counts, maintenance windows, estimated restoration times, and causes may change as new information becomes available.

A published estimate is not a guarantee that work or restoration will be completed at that time.

The absence of a service type, suburb, postcode, state, or other location from a published record does not necessarily confirm that it is unaffected.

This repository is not a replacement for:

* Emergency service information.
* Individual customer support.
* Service-specific fault diagnosis.
* Contractual service-level reporting.
* Formal regulatory notices where a separate notice is required.

Customers experiencing a service issue should use the official support channels provided by their service provider.

In an emergency, contact the appropriate emergency service or authority.

## Licence

Unless otherwise stated, the public outage and maintenance data in this repository is made available under the licence contained in the repository's LICENSE file.



Before publishing the repository, the repository owner should select and include an explicit licence covering:



Reuse of the published data.

Attribution requirements.

Redistribution.

Creation of derivative works.

Any applicable warranty and liability limitations.

Software source code, website assets, documentation, and published outage data may require different licence terms. Where separate terms apply, they should be identified clearly.



## Contact and Support

For service-specific assistance, use the organisation's official customer support channels.



For questions about the public dataset, JSON structure, accessibility, or repository operation, use the contact details published by the organisation or raise a GitHub issue that does not contain sensitive information.

## Repository Status

This repository is a public transparency and historical publication mechanism. It contains published outage and maintenance records and is not the internal source system used to ingest vendor events, correlate services, or manage operational response.

