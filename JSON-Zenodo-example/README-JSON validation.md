# Validating README-JSON files

The validation schema is located at [`schema.json`](../schema.json) and uses JSON Schema Draft 2020-12.

## Validation modes

The schema supports both the distributed template and completed README-JSON files:

- The template is recognized by its top-level `guidelines` array. Empty `value` fields are allowed.
- A completed file has no top-level `guidelines` array. It must provide non-empty values for authors, main contact, dataset license, README version, PID, and brief description.
- The PID must be a valid URI and the main contact must be an email address.
- Optional sections may be removed, and custom properties are allowed.

## Mandatory and optional content

The current schema validates two related forms of the file.

### Mandatory in every file

Every valid file must contain:

- `license`: a non-empty string describing the license for the structure or file;
- `template`: an object containing `basic_information`, `description`, and `metadata`;
- `basic_information` and `description`: objects whose entries, when present, must contain a non-empty `label` and a string `value`;
- `metadata`: an object, which may be empty.

The `placeholder` and `instruction` properties are optional. They provide guidance and do not replace the actual `value`.

### Mandatory in a completed README

When the top-level `guidelines` property is absent, the file is treated as completed. In addition to the requirements above, these fields must exist and have non-empty values:

- `template.basic_information.authors`;
- `template.basic_information.main_contact`;
- `template.basic_information.dataset_license`;
- `template.basic_information.readme_version`;
- `template.basic_information.pid`;
- `template.description.brief_description`.

The main contact must be an email address, and the PID must be a valid URI. A DOI URL is valid, but the schema also accepts other persistent identifier URLs.

### Optional or conditional content

- The top-level `guidelines` array is optional for completed files. If present, it must contain at least one string and the file is treated as the reusable template.
- All other individual description fields are optional, including research context, publications, file descriptions, processing, ethics, informed consent, and sensitive-data sections.
- The `project_website` and other optional basic-information fields may be omitted.
- `metadata` may remain empty, and custom metadata keys are allowed.
- Additional project-specific sections and properties are allowed because the schema is intentionally extensible.

Removing an optional section is valid. An optional field that is present must still follow the field-object structure: it needs a `label` and a string `value`.

## Minimal valid files

The smallest valid **template-mode** file is:

```json
{
	"license": "Apache-2.0",
	"guidelines": ["Fill in the fields and remove these instructions."],
	"template": {
		"basic_information": {},
		"description": {},
		"metadata": {}
	}
}
```

The smallest valid **completed-mode** file must contain the core fields:

```json
{
	"license": "Apache-2.0",
	"template": {
		"basic_information": {
			"authors": {"label": "Author(s)", "value": "Example Author"},
			"main_contact": {"label": "Main contact", "value": "author@example.org"},
			"dataset_license": {"label": "Dataset license", "value": "CC-BY-4.0"},
			"readme_version": {"label": "README version", "value": "_README_v1-0"},
			"pid": {"label": "PID", "value": "https://doi.org/10.1234/example"}
		},
		"description": {
			"brief_description": {
				"label": "Brief Description",
				"value": "Example dataset description."
			}
		},
		"metadata": {}
	}
}
```

These examples describe the minimum accepted by the schema, not the minimum recommended documentation for a publishable dataset. A real README should normally include the applicable file descriptions, variables, processing, limitations, license conditions, and reuse restrictions.

## What good validation should check

Validation should be layered. A file can be valid JSON but still be incomplete or unsuitable for reuse.

1. **Syntax:** Confirm that the file can be parsed as JSON. This catches missing commas, unmatched braces, invalid quotes, and similar errors.
2. **Structure:** Confirm that the JSON follows `schema.json`: required containers exist, fields have the expected types, and values such as email addresses and URIs have the correct format.
3. **Completeness:** For a completed README, confirm that the core identity fields are present and non-empty. Optional sections should only be required when they apply to the dataset.
4. **Semantics:** Review whether the descriptions are accurate and understandable. Check that file names, variables, units, missing-value codes, processing steps, licenses, and restrictions actually match the dataset.
5. **Reproducibility:** Check that the PID resolves, referenced files and publications exist, software versions are recorded where relevant, and another person could understand how the data were created and processed.

The JSON Schema performs the first part of structural validation and the basic completeness checks described above. It cannot determine whether prose is accurate, whether a PID resolves, whether a license is appropriate, or whether the README truly describes the accompanying data. Those checks require a reviewer or a separate validation script.

A useful validation process should:

- report the exact JSON path of every error, for example `template.basic_information.pid.value`;
- distinguish errors that must be fixed from warnings or review reminders;
- validate the schema itself before validating documents;
- fail automated checks for malformed or structurally invalid files; and
- keep human review for meaning, accuracy, permissions, and data quality.

Passing the schema therefore means **structurally valid**, not automatically **complete, accurate, or scientifically reusable**.

## Example with Python

Install a Draft 2020-12-compatible validator:

```text
python -m pip install jsonschema
```

Validate the template:

```python
import json
from pathlib import Path

from jsonschema import Draft202012Validator, FormatChecker

root = Path(__file__).resolve().parents[1]
schema = json.loads((root / "schema.json").read_text(encoding="utf-8"))
document = json.loads((root / "templates" / "README-JSON.json").read_text(encoding="utf-8"))

Draft202012Validator.check_schema(schema)
Draft202012Validator(schema, format_checker=FormatChecker()).validate(document)
print("README-JSON is valid")
```

When validating a completed file, replace the `document` path with the path to that file. Remove the top-level `guidelines` property before validation so the completed-file requirements are applied.

## Validation with Docker

Run these commands from the repository root. Docker Desktop must be running on Windows.

### Windows PowerShell

```powershell
docker run --rm -v "${PWD}:/work" -w /work python:3.12-alpine sh -c "pip install --quiet check-jsonschema && check-jsonschema --schemafile schema.json templates/README-JSON.json"
```

`${PWD}` expands to the current directory in PowerShell. The current directory is mounted into the container as `/work`.

### Windows Command Prompt

```bat
docker run --rm -v "%cd%:/work" -w /work python:3.12-alpine sh -c "pip install --quiet check-jsonschema && check-jsonschema --schemafile schema.json templates/README-JSON.json"
```

`%cd%` expands to the current directory in Command Prompt. Docker Desktop translates the Windows path for the Linux container.

### Linux or macOS

```bash
docker run --rm -v "$(pwd):/work" -w /work python:3.12-alpine sh -c 'pip install --quiet check-jsonschema && check-jsonschema --schemafile schema.json templates/README-JSON.json'
```

`$(pwd)` expands to the current repository directory in a POSIX shell.

### Windows with WSL

Use the Linux or macOS command from a WSL terminal. If the repository is stored on the Windows filesystem, for example under `/mnt/c`, Docker Desktop must have access to that drive.

For a completed README, replace `templates/README-JSON.json` in any command with the path to the completed file. A successful validation prints `ok -- validation done`; otherwise, the output includes the JSON path and the failed rule.
