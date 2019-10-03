# Breeze Guide

## Rational

Were using Synthea to generate patient and patient related data. 

Also check out this for other related tooling

## prerequisites
- [synthea (this repo)](https://github.com/synthetichealth/synthea)
- [blazectl](https://github.com/life-research/blazectl) 
- telepresense: TODO link to documentation

## Other tools

* [bbmr-fhir-gen](https://alexanderkiel.gitbook.io/blaze/tooling#bbmri-fhir-gen:) : produces a smaller less rich set of data


## quick start example

run the following to start telepresence

```
$ telepresence --namespace development
```

Generate seed FHIR data using synthea. If successful you should expect to see output
in the console listing the created files/FHIR bundles.
-p value is the number of "bundles". e.g a patient and his medications, organization, etc...

Run from the root directory of synthea to generate 50 new FHIR paitent bundles.

```
./run_synthea -p 50
```

this produces output synthea/output/fhir.

Now upload to the database using blazectl. run this from the root of directory of synthea or replace directory with relative path

```
$ blazectl upload ./synthea/output/fhir --server http://blaze:8080/fhir
```
