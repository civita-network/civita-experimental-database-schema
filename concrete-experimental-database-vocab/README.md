# Controlled Vocabularies and Parametrization Registries

This folder contains the JSON files used by the Concrete Grand Challenge Experimental Database to define controlled terms, structured parametrizations, and reusable machine-readable metadata for database fields.

The JSON files in this folder are the authoritative source for permitted terms used across the Excel data collection template, JSON validation schemas, LaTeX documentation, example submissions, and contributor-facing webform. Some files only define controlled vocabulary terms, whereas others also include parametrization schemas for selected terms.

## Purpose

Controlled vocabularies provide standardized terms for database fields that would otherwise be difficult to interpret consistently across contributors, laboratories, publications, and experimental traditions.

For example, contributors may describe the same material, test, device, support, loading condition, or measurement quantity using different terminology. The JSON files in this folder define common accepted terms so that equivalent concepts are represented consistently throughout the database.

The files are intended to support:

```text
consistent data entry
automated validation
controlled webform dropdown menus
structured parametrization of selected records
search and filtering
data harmonization
long-term database interoperability
```

## Folder organization

All controlled vocabulary and parametrization JSON files are stored in:

```text
vocab/
```

## Contents

This folder may include files such as:

```text
source_types.json
material_classes.json
cement_types.json
aggregate_types.json
fly_ash_classes.json
superplasticizer_types.json
water_reducer_types.json
air_entrainment_types.json
hydration_accelerator_types.json
rheology_modifier_types.json
fiber_types.json
test_types.json
observed_mechanisms.json
devices_and_supports_library.json
loading_mode_types.json
curing_conditions.json
data_types.json
quantity_reported_types.json
extraction_method_types.json
```

Additional vocabulary or registry files may be added as the database evolves.

## Vocabulary files and registry files

The JSON files in this folder may have two levels of complexity.

A simple vocabulary file answers the question:

```text
What terms are allowed?
```

A parametrization registry answers both questions:

```text
What terms are allowed?
How should selected terms be described in structured form?
```

For example, a simple vocabulary file may define `cement_type` or `material_class` terms. A registry file such as `devices_and_supports_library.json` may define all admissible `device_type` terms and, for selected entries, include a structured parameter schema.

In the devices/supports registry, all device and support types should be listed. Terms without a current parametrization should remain valid controlled vocabulary entries and may use:

```json
{
  "parameter_schema_status": "not_yet_defined",
  "parameters": []
}
```

Terms with an available parametrization should use:

```json
{
  "parameter_schema_status": "defined",
  "parameters": [
    {
      "name": "example_parameter",
      "type": "number",
      "units": "mm",
      "required": true,
      "description": "Description of the parameter."
    }
  ]
}
```

## Relationship to the Excel template

The Excel data collection template uses the JSON files in this folder to define allowed values for selected fields. These fields may appear as dropdown menus, validation lists, or documented allowable entries.

When a vocabulary or registry file is updated, the corresponding spreadsheet fields should be reviewed and updated if needed.

Examples of spreadsheet fields governed by controlled vocabularies or registries include:

```text
source_type
material_class
cement_type
aggregate_type
fly_ash_class
superplasticizer_type
water_reducer_type
air_entrainment_type
hydration_accelerator_type
rheology_modifier_type
fiber_type
test_type
observed_mechanisms
device_type
loading_mode
curing_condition
data_type
quantity_reported
extraction_method
hygral_boundary_condition
thermal_boundary_condition
```

## Relationship to JSON validation schemas

The JSON validation schemas stored in:

```text
validation_schemas/
```

should enforce the controlled terms defined in this folder whenever a field is restricted to a prescribed set of values.

For registry files that include parametrization schemas, validation should proceed in two stages:

1. Check that the submitted term is present in the registry.
2. If the selected term has `parameter_schema_status` equal to `defined`, validate the submitted parameters against the corresponding parameter schema.

Whenever a term or parameter definition is added, removed, renamed, deprecated, or modified, the corresponding validation schema should be reviewed and updated.

## Relationship to the webform

The contributor-facing webform stored in:

```text
webform/
```

should use the JSON files in this folder to populate dropdown menus, radio-button options, multi-select fields, conditional parameter sections, and validation rules.

For example:

```text
material_class              → material class dropdown
cement_type                 → cement type dropdown
aggregate_type              → aggregate type dropdown
test_type                   → test type dropdown
device_type                 → device/support type dropdown
loading_mode                → loading mode dropdown
curing_condition            → curing condition dropdown
data_type                   → data type dropdown
extraction_method           → extraction method dropdown
```

The webform should not define independent lists of terms that duplicate the JSON files in this folder. These files should be treated as the authoritative source for controlled terms and available parametrizations.

For registry files with defined parameters, the webform may use the selected term to display conditional input fields. For example, if `device_type = pad_support`, the webform may display the parameters required by the `pad_support` schema.

## Recommended JSON structure

Each vocabulary or registry file should include, when applicable:

```text
vocabulary_name or library_name
version
description
field_name
naming_convention
entries
```

Each term or entry should include, when applicable:

```text
term
display_name
definition
category
aliases
notes
parameter_schema_status
parameters
example
```

A typical simple vocabulary entry may have the following structure:

```json
{
  "term": "example_term",
  "display_name": "Example Term",
  "definition": "Definition of the controlled vocabulary term.",
  "aliases": [],
  "category": "example_category"
}
```

A typical registry entry with a parametrization may have the following structure:

```json
{
  "term": "example_parametrized_term",
  "display_name": "Example Parametrized Term",
  "definition": "Definition of the controlled vocabulary term.",
  "category": "example_category",
  "parameter_schema_status": "defined",
  "parameters": [
    {
      "name": "example_parameter",
      "type": "number",
      "units": "mm",
      "required": true,
      "description": "Description of the parameter."
    }
  ]
}
```

The exact structure may vary depending on the vocabulary or registry, but all entries should include at least a machine-readable term and a clear definition.

## Naming conventions

JSON file names should use:

```text
lowercase_underscore_separated.json
```

Examples:

```text
material_classes.json
cement_types.json
test_types.json
devices_and_supports_library.json
loading_mode_types.json
```

Machine-readable terms should generally use one of the following conventions, as specified in each file:

```text
lowercase_underscore_separated
standard_designation_underscore_separated
category | mechanism_name
```

Display names may use conventional capitalization and formatting for presentation in documentation and webforms.

## Use of `other` and `unknown`

Many vocabularies include the terms:

```text
other
unknown
```

These terms have distinct meanings.

Use:

```text
other
```

when the relevant concept is known but does not fall within the predefined vocabulary terms.

Use:

```text
unknown
```

when the relevant information is not reported, not identifiable, or cannot be determined from the available source.

When `other` is used, contributors should provide an explanatory note whenever possible.

## Editing guidelines

When editing JSON files in this folder:

1. Preserve valid JSON syntax.
2. Use stable, machine-readable terms.
3. Provide a definition for every term.
4. Avoid renaming existing terms unless necessary.
5. Prefer adding new terms over changing the meaning of existing terms.
6. Use `deprecated` metadata if a term should be phased out.
7. Keep naming conventions consistent within each file.
8. Avoid duplicate or near-duplicate terms.
9. Add parametrization schemas only when the required parameters are sufficiently well defined.
10. Use `parameter_schema_status` to distinguish entries with defined parameters from entries whose parametrization will be added later.
11. Update corresponding documentation when terms or parameters change.
12. Update validation schemas, examples, spreadsheet fields, and webform components when terms or parameters change.

## Adding a new term

Before adding a new term, contributors should verify that the concept is not already represented by an existing term or alias.

A new term should be added only when it is technically meaningful, broadly useful, and sufficiently distinct from existing terms.

A pull request adding a new term should include:

```text
the proposed machine-readable term
a display name
a concise definition
a category, if applicable
aliases, if applicable
a justification for the addition
affected database fields
affected schemas, documentation, examples, spreadsheet fields, and webform components
```

## Adding a parametrization schema

Before adding a parametrization schema for an existing term, contributors should verify that the proposed parameters are general enough to support data from multiple sources and specific enough to support validation and reuse.

A pull request adding a parametrization schema should include:

```text
the term being parametrized
the proposed parameter names
the parameter types
the parameter units, when applicable
which parameters are required and optional
clear descriptions of all parameters
an example record
any affected validation schemas, examples, spreadsheet fields, and webform components
```

Parametrization schemas should use stable names and explicit units. Parameter names should avoid ambiguity and should be consistent with the relevant specimen, geometry, test, or device coordinate system.

## Deprecating a term

Terms should not be removed abruptly if they may already appear in existing data submissions.

Instead, deprecated terms should be clearly marked, and replacement terms should be identified.

A deprecated entry may include:

```json
{
  "term": "old_term",
  "display_name": "Old Term",
  "definition": "Deprecated term retained for backward compatibility.",
  "deprecated": true,
  "replacement": "new_term"
}
```

## Validation

All JSON files in this folder should be validated before merging changes.

At minimum, contributors should check that:

```text
the file is valid JSON
all entries include definitions
terms are unique within each vocabulary or registry
naming conventions are followed
required fields are present
parameter schemas are internally consistent
parameter units are explicit when applicable
parameter names are unique within each parametrized entry
deprecated terms include replacement guidance when applicable
```

Additional automated validation scripts may be provided in:

```text
tools/validation/
```

## Contribution workflow

Proposed changes to JSON files in this folder should be submitted through GitHub Issues and Pull Requests.

A vocabulary or registry-related pull request should indicate whether corresponding updates are required in:

```text
templates/
documentation/
validation_schemas/
webform/
examples/
tools/validation/
```

Before merging, reviewers should verify that the proposed change is technically justified, internally consistent, and compatible with the broader database structure.

## Current status

The controlled vocabularies and parametrization registries are under active development. Terms, definitions, categories, naming conventions, and parameter schemas may change before the first public release of the database specification.

Contributors should use the latest JSON files in this folder unless a specific release version is required.
