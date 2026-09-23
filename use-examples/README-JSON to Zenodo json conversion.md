# Converting README-JSON to Zenodo metadata JSON

The README-JSON file documents the dataset for people who use or preserve it. Zenodo JSON is repository metadata used to create or fill a Zenodo deposition. They overlap, but they are not the same document and should not be copied field-for-field without review.

This guide follows the [Zenodo REST API documentation](https://developers.zenodo.org/#representation) and the [Zenodo create-deposit documentation](https://developers.zenodo.org/#create-a-new-deposit).

## Three JSON formats

Keep these formats separate:

1. `README-JSON.json` contains the project documentation under `template`.
2. `.zenodo.json` is the GitHub release metadata file. It contains Zenodo metadata fields at the top level, without a `metadata` wrapper.
3. A Zenodo REST API request wraps the same metadata inside `metadata`:

```json
{
	"metadata": {
		"title": "Example dataset",
		"upload_type": "dataset",
		"publication_date": "2026-09-23",
		"creators": [{"name": "Surname, Given name"}],
		"description": "Short dataset description.",
		"access_right": "open",
		"license": "cc-by-4.0"
	}
}
```

Do not submit the complete README-JSON file to Zenodo. Extract and transform the relevant values first.

## Zenodo required metadata

For a dataset deposition, prepare at least:

- `title`: the dataset title;
- `upload_type`: normally `dataset`;
- `publication_date`: ISO date in the form `YYYY-MM-DD`;
- `creators`: an array of creator objects, each with a `name`;
- `description`: the dataset description;
- `access_right`: usually `open`, `embargoed`, `restricted`, or `closed`;
- `license`: required for `open` and `embargoed` records and must use a Zenodo license identifier such as `cc-by-4.0`.

The current README-JSON template does not have dedicated fields for `title`, `publication_date`, `upload_type`, or `access_right`. These values must therefore be added manually during conversion or added as project-specific fields in the README-JSON. Never infer them silently.

## Field mapping

| README-JSON value | Zenodo field | Conversion or review needed |
| --- | --- | --- |
| Dataset name in the README heading | `title` | Copy manually; this field is not currently represented in the JSON template. |
| `template.basic_information.authors.value` | `creators` | Convert the text into an array. Confirm name order, affiliation, ORCID, and each creator separately. |
| `template.description.brief_description.value` | `description` | Use as the base description. Add research context and methods when useful. |
| `template.basic_information.dataset_license.value` | `license` | Convert human-readable text such as `CC-BY 4.0` to a Zenodo identifier such as `cc-by-4.0`. Check the available licenses. |
| `template.basic_information.pid.value` | `related_identifiers` or `doi` | Decide whether it identifies the same resource, a previous version, or a related publication. Do not automatically use it as the Zenodo DOI. |
| `template.basic_information.project_website.value` | `related_identifiers` or `references` | Add only if the URL is relevant to the deposited resource. |
| `template.description.related_publications.value` | `related_identifiers` or `references` | Convert each publication into a separate identifier with a relation and resource type where possible. |
| `template.description.method_of_data_processing.value` | `method` | Copy or summarize the processing method. |
| `template.description.collection/creation_of_data.value` | `method` or `dates` | Split methodology from collection dates instead of copying one large text block. |
| `template.description.license_and_terms_of_use.value` | `license` and review notes | Use the Zenodo `license` field for the file license. Keep special conditions in the appropriate access or description fields. |
| Project funding information | `grants` | Add grant identifiers manually in Zenodo format. |
| Keywords identified during review | `keywords` | Create a list of short, useful search terms. Do not treat arbitrary prose as keywords. |
| Community identifier | `communities` | Add the Zenodo community identifier manually. |

The README fields for file descriptions, variables, special values, completeness, restrictions, and software requirements usually remain in the README. They are important documentation, but they do not have a direct one-to-one Zenodo metadata field.

## Recommended conversion workflow

1. Complete the README-JSON and validate it with [schema.json](../schema.json).
2. Remove template instructions and confirm that all required completed fields have meaningful values.
3. Decide whether the output is a GitHub `.zenodo.json` file or a REST API request body.
4. Add the missing Zenodo fields: title, upload type, publication date, access right, and any repository-specific information.
5. Convert creators into structured objects. Do not split names automatically when the original text is ambiguous.
6. Normalize the license to a Zenodo license identifier. Check the available identifiers through the [Zenodo licenses API](https://developers.zenodo.org/#licenses).
7. Convert PIDs and related publications into `related_identifiers` with the correct relation, identifier scheme, and resource type.
8. Validate the resulting JSON locally.
9. For API workflows, create or update an unpublished draft in the Zenodo sandbox first. Review the server response and only publish after human review.

## Example `.zenodo.json`

This is the format used by GitHub release integration. It has no `metadata` wrapper:

```json
{
	"title": "Example environmental dataset",
	"upload_type": "dataset",
	"publication_date": "2026-09-23",
	"creators": [
		{
			"name": "Novak, Jana",
			"affiliation": "Example University",
			"orcid": "0000-0002-1825-0097"
		}
	],
	"description": "This dataset contains measurements collected for the example project. It is intended for reuse and verification of the reported analyses.",
	"access_right": "open",
	"license": "cc-by-4.0",
	"keywords": ["environmental data", "example dataset"],
	"related_identifiers": [
		{
			"identifier": "https://example.org/project",
			"relation": "isSupplementTo",
			"scheme": "url",
			"resource_type": "other"
		}
	]
}
```

## Example REST API request

The REST API uses the same metadata inside a `metadata` object. Creating a deposition requires authentication with a personal access token. Use the sandbox while testing; it uses a separate account and token from production.

```python
import os
import requests

metadata = {
		"title": "Example environmental dataset",
		"upload_type": "dataset",
		"publication_date": "2026-09-23",
		"creators": [{"name": "Novak, Jana"}],
		"description": "This dataset contains measurements collected for the example project.",
		"access_right": "open",
		"license": "cc-by-4.0"
}

token = os.environ["ZENODO_TOKEN"]
response = requests.post(
		"https://sandbox.zenodo.org/api/deposit/depositions",
		json={"metadata": metadata},
		headers={"Authorization": f"Bearer {token}"},
		timeout=30,
)
response.raise_for_status()
print(response.json()["links"]["self"])
```

This creates a draft deposition. Do not call the publish endpoint until the metadata and uploaded files have been reviewed. Never store an access token in this repository or commit it to a JSON file.

## Validating Zenodo JSON

Validation should happen at three levels:

1. **JSON syntax:** the file must parse as JSON.
2. **Zenodo structure:** fields must have the expected types and controlled values.
3. **Repository acceptance:** Zenodo must accept the metadata for the selected upload type, access right, license, and related fields.

### Local syntax check

```bash
python -m json.tool .zenodo.json
```

### Structural validation with Zenodo's schema

Zenodo publishes a [legacy deposit JSON Schema](https://github.com/zenodo/zenodo/blob/master/zenodo/modules/deposit/jsonschemas/deposits/records/legacyrecord.json). Download a reviewed copy of that schema, then validate the GitHub-style file:

```bash
curl -fsSL "https://raw.githubusercontent.com/zenodo/zenodo/master/zenodo/modules/deposit/jsonschemas/deposits/records/legacyrecord.json" -o zenodo-schema.json
python -m pip install check-jsonschema
check-jsonschema --schemafile zenodo-schema.json .zenodo.json
```

The schema is useful for catching wrong types, missing fields, and invalid controlled values. It is not a guarantee that the current Zenodo API will accept every value, because repository rules can change and some requirements are conditional.

### Validation with Docker

Linux/macOS/WSL:

```bash
docker run --rm -v "$(pwd):/work" -w /work python:3.12-alpine sh -c 'pip install --quiet check-jsonschema && wget -qO zenodo-schema.json https://raw.githubusercontent.com/zenodo/zenodo/master/zenodo/modules/deposit/jsonschemas/deposits/records/legacyrecord.json && check-jsonschema --schemafile zenodo-schema.json .zenodo.json'
```

Windows PowerShell:

```powershell
docker run --rm -v "${PWD}:/work" -w /work python:3.12-alpine sh -c "pip install --quiet check-jsonschema && wget -qO zenodo-schema.json https://raw.githubusercontent.com/zenodo/zenodo/master/zenodo/modules/deposit/jsonschemas/deposits/records/legacyrecord.json && check-jsonschema --schemafile zenodo-schema.json .zenodo.json"
```

### API or sandbox validation

The most authoritative test is to submit the metadata to an unpublished sandbox deposition. Zenodo returns HTTP `201` when a new draft is created. A validation failure usually returns HTTP `400` with an `errors` array containing fields such as `metadata.access_right`, `metadata.creators.0.name`, or `metadata.license`.

Use the sandbox for testing and inspect the response before uploading or publishing files. A successful local schema check does not replace this server-side check or human review.

## Final review checklist

- The JSON is syntactically valid.
- The output format is correct: top-level `.zenodo.json` metadata or API `{"metadata": {...}}`.
- Title, upload type, publication date, creators, description, and access right are present.
- The license is a Zenodo identifier and matches the intended reuse conditions.
- Creator names, affiliations, and ORCID identifiers were checked manually.
- Related identifiers use the correct relation and resource type.
- The README and Zenodo metadata describe the same files, version, and dataset.
- The file passes the local schema check and a sandbox draft test before publication.
