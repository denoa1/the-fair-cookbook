> :bulb: __WIP__

So the recipe draft will include:

- what is the BioSD bioschema profile
- how to use the BioSD tools to generate a bioschema JSON-LD
- how to extend the BioSD generic profile to more specific ones based on BioSD, e.g. EBiSC-BioSD profile
 - how to reuse BioSD pipeline to implement bioschema, without interacting with BioSD

# Using biosamples bioschema implementation, an examplar  with EBiSC


# Table of Contents
1. [Main FAIRification Objectives](#Main%20FAIRification%20Objectives)
2. [Graphical Overview of the FAIRification Recipe Objectives](#Graphical%20Overview%20of%20the%20FAIRification%20Recipe%20Objectives)
3. [FAIRification Objectives, Inputs and Outputs](#FAIRification%20Objectives,%20Inputs%20and%20Outputs)
4. [Capability & Maturity Table](#Capability%20&%20Maturity%20Table)
5. [Table of Data Standards](#Table%20of%20Data%20Standards)
6. [Executable Code in Notebook](#Executable%20Code%20in%20Notebook)
7. [How to create workflow figures](#How%20to%20create%20workflow%20figures)
8. [License](#License)

---

## Main Objectives

The main purpose of this recipe is:

> - 

## Graphical Overview of the FAIRification Recipe Objectives


___
## User Stories

## Capability & Maturity Table

| Capability  | Initial Maturity Level | Final Maturity Level  |
| :------------- | :------------- | :------------- |
| - | 0 | - |

----

## FAIRification Objectives, Inputs and Outputs

| Actions.Objectives.Tasks  | Input | Output  |
| :------------- | :------------- | :------------- |
| [validation, eg](http://edamontology.org/operation_2428)  | [Structure Data File (SDF),eg](https://fairsharing.org/FAIRsharing.ew26v7)  | [report,eg](http://edamontology.org/data_2048)  |



## Table of Data Standards

| Data Formats  | Terminologies | Models  |
| :------------- | :------------- | :------------- |
| [jsonld](htt)  | |


___



## What is BioSchema and why using BioSchema
https://bioschemas.org/profiles/
"The Bioschemas' profiles define a community agreed layer over the Schema.org vocabulary, providing additional constraints. These constraints capture (i) the information properties agreed by the community which are minimum (M), recommended (R), or optional (O), (ii) the cardinality of the property, i.e. whether it is expected to occur once or many times, and (iii) associated controlled vocabulary terms drawn from existing ontologies."

## Sample profile in BioSchema

- [ ] a table of sample profile attributes
- [ ] a screenshot of bioschema sample page
- [ ] a mermaid graph explaining the sample profile structure

meta from https://bioschemas.org/useCases/Sample/

Sample profile version (v0.2-RELEASE-2018_11_10)
10 November 2018 

original from https://docs.google.com/spreadsheets/d/122y9X5ZxFN5sPDX63ox8UGQlHkddT6O69_e2b_eAv5Y/edit#gid=292464567

|                                                                                                                              bioschemas                                                                                                                              |             |             |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------:|:-----------:|
|                                                                                                                            BSC Description                                                                                                                           | Marginality | Cardinality |
| A property-value pair representing an additional characteristics of the entity, e.g. “Organism: Homo sapiens” or “tissue type: leaf”. For details of how to use [PropertyValue](http://bioschemas.org/specifications/Sample/specification/#PropertyValue) see below. | Optional    | MANY        |
|                                         A description of the sample in free text. This should not contain information that could be better expressed as key/value pairs. These should be expressed using additionalProperty.                                         | Optional    | ONE         |
|                                                                  Unique sample IDs. Where possible this should be an identifiers.org compliant prefixed id e.g. a BioSamples ID biosample:SAME12345.                                                                 | Minimum     | MANY        |
|                                                          A human readable name for the sample. This should not be an additional identifier. Additional identifiers should be added to the identifier field.                                                          | Optional    | MANY        |
|                                                                 This is used by validation tools to indentify the profile used. You must use the value specified in the Controlled Vocabulary column.                                                                | Minimum     | ONE         |
|                                                                                               Provides a link to a dataset that contains data about this sample record.                                                                                              | Optional    | MANY        |
|                                                                                           An access URL for this sample, e.g., in BioSamples or in a Biobank or elsewhere.                                                                                           | Recommended | ONE         |



Sample profile from Bioschema.org https://gist.github.com/kcmcleod/bf09c3ce3d6916dea793d6c4e7a1087c
```jsonld
{
	"@context": "http://schema.org",
	"@type": ["BioChemEntity", "Sample"],
	"identifier": "biosample:SAMEA104383111",
	"name": "HPSI0617pf-tuji",
	"description": "Fibroblast cell line ...",
	"url": "http://www.ebi.ac.uk/biosamples/samples/SAMEA104383111",
	"dataset": [...],
	"additionalProperty": [{
		"@type": "PropertyValue",
		"name": "cellType",
		"value": "fibroblast from the user",
		"valueReference": {
			"@type": "CategoryCode",
			"name": "fibroblast",
			"url": "http://purl.obolibrary.org/obo/CL_0000057",
			"codeValue": "CL:0000057"
		}
	}]
}
```

## Sample BioSchema profile implementation in BioSamples

Example data:[EBiSC STBCi190-A](https://www.ebi.ac.uk/biosamples/samples/SAMEA104616304

![](https://i.imgur.com/623842U.jpg)

BSD jsonld:
```jsonld
{
	"@context": ["http://schema.org", {
		"OBI": "http://purl.obolibrary.org/obo/OBI_",
		"biosample": "http://identifiers.org/biosample/"
	}],
	"@type": "DataRecord",
	"@id": "biosample:SAMEA104616304",
	"identifier": "biosample:SAMEA104616304",
	"dateCreated": "2017-12-06T00:00:00Z",
	"dateModified": "2019-07-23T21:58:50.245Z",
	"mainEntity": {
		"@id": "https://www.ebi.ac.uk/biosamples/samples/SAMEA104616304",
		"@type": ["Sample", "OBI:0000747"],
		"identifier": ["biosample:SAMEA104616304"],
		"name": "STBCi190-A",
		"url": "https://www.ebi.ac.uk/biosamples/samples/SAMEA104616304",
		"additionalProperty": [{
			"@type": "PropertyValue",
			"name": "cell type",
			"value": "induced pluripotent stem cell",
			"valueReference": [{
				"@id": "http://www.ebi.ac.uk/efo/EFO_0004905",
				"@type": "DefinedTerm"
			}]
		}, {
			"@type": "PropertyValue",
			"name": "donor id",
			"value": "SAMEA104616305"
		}, {
			"@type": "PropertyValue",
			"name": "material",
			"value": "cell line",
			"valueReference": [{
				"@id": "http://www.ebi.ac.uk/efo/EFO_0000322",
				"@type": "DefinedTerm"
			}]
		}, {
			"@type": "PropertyValue",
			"name": "organism",
			"value": "Homo sapiens",
			"valueReference": [{
				"@id": "http://purl.obolibrary.org/obo/NCBITaxon_9606",
				"@type": "DefinedTerm"
			}]
		}, {
			"@type": "PropertyValue",
			"name": "project",
			"value": "EBiSC"
		}, {
			"@type": "PropertyValue",
			"name": "synonym",
			"value": "SFC259-03-01"
		}],
		"sameAs": "http://identifiers.org/biosample/SAMEA104616304"
	},
	"isPartOf": {
		"@type": "Dataset",
		"@id": "https://www.ebi.ac.uk/biosamples/samples"
	}
}
```

## How to download bioschema from BioSamples

### UI, Highlighted in yellow
![](https://i.imgur.com/YKyLvDp.png)

## API
append `.ldjson`
```sh
wget https://www.ebi.ac.uk/biosamples/samples/SAMEA104616304.ldjson
```

## How to use the BioSamples jsonld

- google programmable search example

## next step from BSD implementation
- [ ] whart's not covered in the profile, eg external references, structured data. 
- [ ] profile very generic

## How to create a new profile
## How to reuse the BSD jsonld converter
- [ ] Defining new profile
- [ ] Converting data to new profile






## Executable Code in Notebook

```python
dfsfds
```

## Related recipes

## References
:octopus: dsffs
:octopus: fss


## Authors:

| Name | Affiliation  | orcid | CrediT role  |
| :------------- | :------------- | :------------- |:------------- |
| Fuqi Xu|

___


## License:

<a href="https://creativecommons.org/licenses/by/4.0/"><img src="https://mirrors.creativecommons.org/presskit/buttons/80x15/png/by-sa.png" height="20"/></a>
