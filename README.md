# Islandora DOI Framework

## Overview

Utility module that provides a framework for other modules to assign DOIs ([Digital Object Identifiers](https://en.wikipedia.org/wiki/Digital_object_identifier)) to objects. This module provides the following:

* a "DOI" subtab under each object's "Mangage" tab
* four Drupal hooks
  * one for minting a DOI using an external API
  * one for persisting a DOI, for example in a datastream or database table
  * one for updating a DOI
  * one for checking for the presence of a DOI in a datastream or other location
* a "Assign DOIs to Islandora objects" permission
* a Drush script for adding a DOI to a list of objects

This module differs from the [Islandora DOI](https://github.com/Islandora/islandora_scholar/tree/7.x/modules/doi) module bundled with Islandora Scholar in that this module and its submodules only create new DOIs and manage updating the data associated with a DOI. Scholar's DOI module provides a way for the creation of new objects from a list of DOIs.

## Requirements

* [Islandora](https://github.com/Islandora/islandora)
* A submodule to mint the DOIs, such as the included [DataCite](modules/islandora_doi_datacite) module
* A submodule to persist the DOI locally, such as the included [MODS](modules/islandora_doi_mods) module or Alex Garnett's [DDI DOI](https://github.com/axfelix/islandora_doi_ddi) module

## Installation

Same as for any other Drupal module.

## Configuration

This module does not have any configuration settings of its own. All settings are managed by submodules. It does provide a single permission, "Assign DOIs to Islandora objects", which enables users to access the "DOI" subtab under each object's "Manage" tab.

## Submodules

As described above, submodules are responsible for minting (generating) a DOI (typically, via an API provided by an external organization), for persisting it (typically in a datastream in each object), and for performing any updates to the metadata or URL associated with the DOI. One or more submodules together handle the combination of tasks required to mint a DOI from a specific source and then to persist it in a specific place associated with the Islandora object. The Islandora DOI Framework module defines hooks for accomplishing each of those tasks. These hooks are documented in the `islandora_doi_framework.api.php` file and are illustrated in the included [DataCite](modules/islandora_doi_datacite) and [MODS](modules/islandora_doi_mods) submodules (and sample/test submodules). Note that all hooks do not need to be implemented in the same module; in fact, separating the DOI minting functionality and the DOI persisting functionality in separate modules is preferred to allow implementers to mix and match.

Two additional submodules are available that are intended be used during the development and testing of minting and persisting modules:

* A submodule to mint sample DOIs using a dummy DOI prefix, [Islandora DOI Framework Sample Mint](modules/islandora_doi_framework_sample_mint)
* A submodule to persist DOIs to a text file, [Islandora DOI Framework Sample Persist](modules/islandora_doi_framework_sample_persist)

Note that you should only enable a single minting submodule and a single persisting submodule. Therefore, if you enable one or both of these sample submodules, be sure to disable them before enabling your production minting and persisting submodules.

## Assigning DOIs to lists of objects

This module provides a Drush command to assign DOIs to a list of objects identified in an input file. The PID file is a simple list of object PIDS, one PID per line, like this:

```
islandora:23
islandora:29
// Comments can be prefixed by // or #.
islandora:107
islandora:2183
```

The command (using a file at `/tmp/pids.txt` containing the above list) is:

`drush islandora_doi_framework_assign_dois --user=admin --objects=/tmp/pids.txt`

Note that this feature is not yet complete, but will be soon.

## Maintainer

* [Mark Jordan](https://github.com/mjordan)

## Development and feedback

Pull requests against this module are welcome, as are submodules (suggestions below). Please open an issue in this module's Github repo before opening a pull request.

Submodules that mint DOIs from other [registration agencies](http://www.doi.org/registration_agencies.html) are particularly welcome, as are submodules for persisting DOIs to non-MODS datastreams or other locations.

## To do

* Complete the Drush script used to assign batches of DOIs.
* Add the ability to update DataCite metadata and object URLs associated with DOIs. Figure out best trigger and workflow for updating metadata. This should probably not happen every time the source datastream is modified, although that is one option. Maybe a second button under the "DOI" tab for updating metadata? Could also have an associated drush commnand.
* Submodules that mint DOIs from registration agencies other than DataCite.
* Submodules that persist DOIs to locations other than MODS.

## License

 [GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)

