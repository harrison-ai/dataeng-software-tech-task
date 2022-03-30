# DICOM CLI Programming Task

This is a technical assessment task for the [Software Engineer - Data Engineering Team](
https://jobs.lever.co/harrison/3fa6cc4e-acd4-4e82-9ba0-224524cb4c6a) role at [harrison.ai](
https://harrison.ai).

Your task is to develop a library and command-line tool for filtering large sets of DICOM files based
on caller-provided filtering criteria.

You may implement it either as a Python Package *or* a Rust Crate, and are welcome to use external
open-source libraries if desired.


## Task Expectations

You should take a copy of this repo, implement the requirements described below, and push them to GitHub
for discussion in the scheduled technical interview. You're welcome to use a private repository, as
long as it is visible to `@doc-E-brown`, `@oliverdaff`, `@rfk` and `@timleslie`.

You will be given the email address of a member of the team, and should email this person with a link to your
solution at least 24 hours before the scheduled technical interview. You're also encouraged to email them with
questions at any time while working on this task.

**Please don't spend more than a few hours on this task**.

It's not a trick question and we're not expecting anything too elaborate. It's a small self-contained exercise to
help us get to know you through your code - how you break down a problem, how you structure your code, and how you
communicate about it with your potential future team-mates.

We'll be looking for well-structured and documented code, that is clear and maintainable with reasonable
performance characteristics. We will *not* be looking for micro-optimisations or for every single detail to be
carefully polished.

## About DICOM

Digital Imaging and Communications in Medicine (DICOM) is the standard format for sharing medical image information,
particularly in the field of radiology. As a world-leading AI company in radiology we need to be able to efficiently
process de-identified and decomposed DICOM files on a daily basis.

DICOM files are represented in JSON format as a collection of typed key-value pairs. Each *tag* in the DICOM has a
corresponding list of *values* as well as a *value representation* that determines how those values should be
interpreted. For example:

```json
{
    "tag_number_1":{
        "Value":[
            "the value for the tag"
        ],
        "vr":"TypeCode"
    },
    "tag_number_2":{
        "Value":[
            "the value for the tag"
        ],
        "vr":"TypeCode"
    }
}
```

You can find some example files with DICOM JSON data in the `./example-data` directory.

## Requirements

Your library should provide a function for checking whether a DICOM matches a list of tag/value pairs. For example,
given a DICOM with the following tags in JSON format:

```json
    "00080005":{
        "Value":[
            "ISO_IR 100"
        ],
        "vr":"CS"
    },
    "00080008":{
        "Value":[
            "DERIVED",
            "PRIMARY"
        ],
        "vr":"CS"
    },
    "00080016":{
        "Value":[
            "1.2.840.10008.5.1.4.1.1.1.1"
        ],
        "vr":"UI"
    },
    "20500020":{
        "Value":[
            "IDENTITY"
        ],
        "vr":"CS"
    }
```

Then this filter criteria should be considered a match:

```
[("00080005", "ISO_IR 100"), ("00080016", "1.2.840.10008.5.1.4.1.1.1.1")]
```

While this filter criteria should not be considered a match:

```
[("00080005", "ISO_IR 72"), ("00080016", "1.2.840.10008.5.1.4.1.1.1.1")]
```

Your command-line tool should accept filtering criteria as command-line arguments, traverse a directory
containing DICOM files in JSON format, and print the files that match the given criteria. For example:

```bash
$ dcm_filter --keys="00080005,00080016" --values="ISO_IR 100,1.2.840.10008.5.1.4.1.1.1.1" ./example-data/
./example-data/dicom1.json
./example-data/subdir/dicom3.json
```

This functionality may be implemented as either a **Python Package** or a **Rust Crate**, and may use external
open-source libraries if desired.
