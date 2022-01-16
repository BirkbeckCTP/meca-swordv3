# MECA/SWORDv3 and Janeway

This repository is the initial specification space for Janeway's work on MECA/SWORDv3.

MECA is the [Manuscript Exchange Common Approach](https://www.niso.org/publications/rp-30-2020-meca), a NISO standard for handling the exchange of manuscripts between different manuscript workflow systems.

[SWORD 3.0](https://swordapp.github.io/swordv3/swordv3.html) "is a protocol enabling clients and servers to communicate around complex digital objects, especially with regard to supporting the deposit of these objects into a service like a digital repository. Complex digital objects consist of both Metadata and File content, where the Files may be in a variety of formats, there may be many files, and some may be very large. The protocol defines semantics for creating, appending, replacing, deleting, and retrieving information about these complex resources. It also enables servers to communicate regarding the status of treatment of deposited content, such as exposing ingest workflow information."

The MECA/SWORDv3 working group seeks to fuse the approaches of these two standards, specifically to eradicate the FTP dependency in MECA.

## Janeway Use Cases
The most prominent use cases for Janeway to interact with MECA/SWORDv3 are:

* Automatic deposit of published articles in external institutional or subject repositories (may require an external registry of such repositories)
* Transferring content that has been peer-reviewed to or from another instance or MECA/SWORDv3 system (e.g. an OJS install)
* Transferring a preprint to or from another instance or MECA/SWORDv3 system (e.g. an OJS install)

Janeway could, then, act as both a MECA/SWORDv3 client and server. As a server, it could accept offsite exchanges/interactions. As a client, it could push manuscripts out.

## SWORDv3 expression
SWORD objects are JSON files and specifically a 'status document' that is returned whenever an operation is made on a SWORD object. The status document makes reference to and passes a set of URLs that contain metadata file paths. For example, the fileset item in the JSON looks like this:

    "fileSet" : {
        "@id" : "http://www.myorg.ac.uk/sword3/object/1fileset",
        "eTag" : "..."
    },

The SWORD server as a whole can also return a 'service document' that 'defines the capabilities and operational parameters of the server as a whole, or of a particular Service-URL'.

## Existing SWORDv3 Python clients
[SWORD already provides an existing Python client](https://sword3-clientpy.readthedocs.io/en/latest/). There is also [a common library](https://github.com/swordapp/sword3-common.py) that implements basic object structures for both client and server implementations. There seems to be no reference server implementation at present.

## MECA Specification
The MECA specification transfers ZIP files between instances, using the naming format {UUID}-meca.zip where {UUID} is a type 1 UUID. The zip file must contain:

1. A "manifest.xml" file that validates against the MECA Manifest DTD (the MECA metadata format).
2. A "transfer.xml" file that validates against the MECA Transfer DTD (the MECA sending/receiving transfer metadata format).
3. An "article.xml" file that provides JATS XML or a JATS metadata stub.
4. An optional "reviews.xml" file that validates against the MECA Reviews DTD (the MECA reviews metadata format).
5. Content files that contain all the files that make up the submission (e.g. the author's manuscript, any production files).

# Implementation
Our proposed implementation can unfold over two stages:

1. Creation of MECA-compatible zip files for articles.
2. Layering this transfer on top of SWORDv3.

The first stage is by far the easier part as it involves adding a metadata-generating endpoint. The second phase, transferring data by SWORD between endpoints is trickier. Even the lack of a reference server implementation means that we are reliant on building a basic client-server architecture for development/testing.


Martin Paul Eve, January 2022