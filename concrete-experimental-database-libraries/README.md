# JSON Libraries

This folder contains JSON library files used by the Concrete Grand Challenge Experimental Database.

The library files define reusable, structured descriptions of common experimental entities, parametrizations, and configurations used across data submissions. They are intended to support consistency among the Excel data collection template, LaTeX documentation, JSON validation schemas, controlled vocabularies, and contributor-facing webform.

## Purpose

The purpose of the `libraries/` folder is to provide machine-readable reference definitions for database entities that require structured parametrization beyond a simple controlled vocabulary term.

Controlled vocabularies define permitted terms, such as `cube`, `cylinder`, `load_cell`, or `roller_support`. JSON libraries define the associated parameters, units, coordinate conventions, metadata fields, and expected structure required to describe those terms in a complete and reusable way.

For example, a vocabulary may define that `cylinder` is an allowed geometry type, while the corresponding geometry library defines the parameters needed to describe a cylinder, such as diameter, height, reference axes, and dimensional units.

## Contents

This folder may include library files such as:

```text
geometry_library.json
devices_and_supports_library.json
reinforcement_library.json
test_setup_library.json
printing_variables_library.json
loading_protocol_library.json
```

Additional library files may be added as the database structure evolves.

## Relationship to the database hierarchy

The library files support reusable definitions associated with the main database entities. These include, but are not limited to:

```text
geometries
reinforcement_materials
reinforcement_layouts
reinforcement_components
printing_variables
devices_and_supports
loading_histories
tests
```

The libraries are not submitted experimental data by themselves. Rather, they define the structure and parameters used to describe experimental entities consistently across submissions.

For example:

```text
geometry_library.json
```

defines the allowed parametrizations of specimen geometries used in the `geometries` tab.

```text
devices_and_supports_library.json
```

defines the expected parameters for supports, sensors, fixtures, actuators, and measurement devices used in the `devices_and_supports` tab.

```text
reinforcement_library.json
```

defines the expected structure for internal and external reinforcement components, reinforcement layouts, and associated parameters.

## Difference between vocabularies and libraries

The repository distinguishes between controlled vocabularies and JSON libraries.

Controlled vocabularies are stored in:

```text
vocab/
```

They define permitted terms for specific database fields. Examples include:

```text
material_class
cement_type
test_type
device_type
loading_mode
data_type
extraction_method
```

JSON libraries are stored in:

```text
libraries/
```

They define structured parametrizations associated with selected terms. For example:

| Concept            | Vocabulary role                          | Library role                                            |
| ------------------ | ---------------------------------------- | ------------------------------------------------------- |
| Geometry type      | Defines allowed geometry names           | Defines required geometry parameters                    |
| Device type        | Defines allowed device/support names     | Defines device-specific parameters                      |
| Reinforcement type | Defines allowed reinforcement categories | Defines component and layout parameters                 |
| Test setup         | Defines allowed test categories          | Defines setup-specific geometric and loading parameters |

In general, a vocabulary answers the question:

```text
What terms are allowed?
```

A library answers the question:

```text
How should a selected term be described?
```

## Relationship to JSON validation schemas

The library files are closely related to the JSON validation schemas stored in:

```text
validation_schemas/
```

The validation schemas define formal rules for submitted data, including required fields, allowed data types, permitted values, and structural constraints.

The library files define the expected content and parametrization logic used by those schemas.

Whenever a library file is modified, the corresponding validation schema should be reviewed and updated if needed. Conversely, changes to validation schemas may require updates to one or more library files.

## Relationship to the Excel template

The Excel data collection template stored in:

```text
templates/
```

uses the library files conceptually to define how contributors should describe entities such as geometries, reinforcement layouts, supports, sensors, and loading configurations.

When a library file is updated, the relevant spreadsheet tabs and columns should be reviewed for consistency. In particular, changes to geometry, reinforcement, device, support, or test-setup parametrizations may require updates to the corresponding spreadsheet tabs.

## Relationship to the webform

The contributor-facing webform stored in:

```text
webform/
```

should use the same library definitions to generate or validate structured form fields.

For example, selecting a geometry type in the webform should expose the parameters defined in `geometry_library.json`. Selecting a device or support type should expose the parameters defined in `devices_and_supports_library.json`.

Therefore, library changes may require updates to:

```text
webform schemas
webform components
webform pages
API routes
validation schemas
documentation
examples
```

## Recommended JSON structure

Each library file should include, when applicable:

```text
library_name
version
description
applicable_database_entity
naming_convention
entries
```

Each entry should include, when applicable:

```text
term
display_name
definition
category
required_parameters
optional_parameters
units
coordinate_convention
notes
```

A typical entry may have the following conceptual structure:

```json
{
  "term": "example_term",
  "display_name": "Example Term",
  "definition": "Definition of the library entry.",
  "category": "example_category",
  "required_parameters": [
    {
      "name": "parameter_name",
      "unit": "mm",
      "type": "number",
      "definition": "Definition of the parameter."
    }
  ],
  "optional_parameters": [
    {
      "name": "optional_parameter_name",
      "unit": "dimensionless",
      "type": "string",
      "definition": "Definition of the optional parameter."
    }
  ],
  "notes": "Additional implementation notes."
}
```

The exact structure may vary depending on the library type, but field names should remain consistent wherever possible.

## Naming conventions

Library files should use consistent naming conventions.

Recommended file naming:

```text
lowercase_underscore_separated.json
```

Examples:

```text
geometry_library.json
devices_and_supports_library.json
reinforcement_library.json
test_setup_library.json
```

Recommended JSON key naming:

```text
lowercase_underscore_separated
```

Examples:

```text
geometry_type
required_parameters
optional_parameters
coordinate_convention
reference_point
parameter_name
parameter_value
```

## Units

All dimensional quantities should include explicit units.

Preferred units should be consistent with the database documentation and validation schemas. Common units include:

```text
mm
m
N
kN
MPa
GPa
s
min
h
days
deg
dimensionless
```

Units should not be embedded in parameter names unless already required by an established spreadsheet column convention. Prefer separate fields for parameter name, value, and unit when possible.

## Coordinate conventions

Library entries describing geometry, reinforcement, devices, supports, sensors, or loading configurations should define coordinate conventions clearly.

When applicable, each library entry should specify:

```text
reference point
axis orientation
location parameters
orientation parameters
sign convention
dimensional units
```

This is particularly important for geometries, notches, reinforcement layouts, supports, sensors, and loading points.

## Editing guidelines

When editing files in this folder:

1. Preserve valid JSON syntax.
2. Use stable, descriptive, machine-readable keys.
3. Avoid changing existing terms unless the change has been reviewed.
4. Add new entries rather than redefining existing entries when possible.
5. Provide definitions for all new entries.
6. Specify required and optional parameters clearly.
7. Include units for all dimensional parameters.
8. Maintain consistency with controlled vocabularies in `vocab/`.
9. Maintain consistency with validation schemas in `validation_schemas/`.
10. Update documentation and examples when library structures change.

## Validation

All JSON files in this folder should be validated before merging changes.

At minimum, contributors should check that:

```text
the file is valid JSON
all required top-level fields are present
all entries contain definitions
all parameter names are unique within each entry
units are specified where applicable
terms are consistent with the corresponding vocabulary files
```

Additional automated validation scripts may be provided in:

```text
tools/validation/
```

## Contribution workflow

Proposed changes to library files should be submitted through GitHub Issues and Pull Requests.

A pull request modifying a library file should indicate whether corresponding updates are required in:

```text
vocab/
validation_schemas/
templates/
documentation/
webform/
examples/
```

Before merging, reviewers should verify that the proposed change is technically meaningful, internally consistent, and compatible with the broader database structure.

## Current status

The JSON libraries are under active development. Library entries, parameter names, coordinate conventions, and validation rules may change before the first public release of the database specification.

Contributors should use the latest version of the files in this folder unless a specific release version is required.

