# Breeze Guide

Explains how to

* generate patient data and related records
* upload that data
* delete that data
* how we can use this process in development

## Rational

Were using Synthea to generate FHIR Patient and related data. Its mission statment summarizes perfectly what we need.

> SyntheaTM is an open-source, synthetic patient generator that models the medical history of synthetic patients. Our mission is to provide high-quality, synthetic, realistic but not real, patient data and associated health records covering every aspect of healthcare.

Another strategy might be to use something like a data generator via clojure test check. But this
would require a significant time investment to even get random data. 


## overview of synthea

Synthea is a framework and its usable in two ways:

1. as a command line tool - which is how i just generated the data. Its super easy and seems (so far) to provide a lot of the relationships we need by default.
2. By configuration. https://github.com/synthetichealth/synthea/wiki/Generic-Module-Framework#relevant-files-and-paths
The configuration data changes how the FHIR bundles is produced. There is even a "non programmer friendly GUI" to create the config files. I figure this might come in handy later, but right now i dont know what im missing with the default generated data so im not looking into it.

## Example use cases

- generate data for use in development to populate the database (blaze)
- generate data for testing on the front end (cabotage) directly

## prerequisites to generate and upload data to blaze
- [synthea (this repo)](https://github.com/synthetichealth/synthea)
- [blazectl](https://github.com/life-research/blazectl) 
- networking proxy aka telepresense: TODO link to documentation
- a running blaze (or similar) db instance


## Instructions

### Upload

run the following to start telepresence

```
$ telepresence --namespace development
```

Generate seed FHIR data using synthea. If successful you should expect to see output
in the console listing the created files/FHIR bundles.
-p value is the number of "bundles". e.g a patient and his medications, organization, etc...

Run from the root directory of synthea to generate 50 new FHIR paitent bundles.

```
# example producing 50 patients.

./run_synthea -p 50
```

this produces output in synthea/output/fhir.

Now upload to the database using blazectl. run this from the root of directory of synthea or replace directory with relative path

```
$ blazectl upload ./synthea/output/fhir --server http://blaze:8080/fhir
```

### Delete

TODO document how to delete data

### Version control

When we produce the FHIR patient data and upload it we should also git commit the files that produced the data.
This will allow us to 

## Other tools

Also check out [blazes docs](https://alexanderkiel.gitbook.io/blaze/tooling#bbmri-fhir-gen) for related tooling

* [bbmr-fhir-gen](https://alexanderkiel.gitbook.io/blaze/tooling#bbmri-fhir-gen:) : produces a smaller less rich set of data

## Improvement ideas

- Ideally this would be doable from the repl, but the current tools are all command line based.

