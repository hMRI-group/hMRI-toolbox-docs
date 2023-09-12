# Getting Started

Before using the hMRI Toolbox, please read the [README.md]({{config.repo_url}}/blob/master/README.md)
and [LICENSE]({{config.repo_url}}/blob/master/LICENSE) files.

## Software requirements

### Matlab

You'll need Matlab to run the hMRI Toolbox. Matlab versions from release R2018a (MATLAB 9.4) and up have been used to
develop and test the software. Some functionality may still work in earlier versions of Matlab, but this is not
officially supported. Please note that a single Matlab version should be used to process all of a given dataset, as
results can differ slightly between versions due to changes in Matlab's internal algorithms.

### SPM

The hMRI Toolbox relies on SPM functionalities and has been developed starting
from [SPM12 (version 12.3 - r6906)](http://www.fil.ion.ucl.ac.uk/spm/software/spm12/). It is recommended to have
the [latest release of SPM12](http://www.fil.ion.ucl.ac.uk/spm/software/spm12/) at hand and to make sure you have added
the SPM root directory to your Matlab Path.

### Git

For users who wish to keep their version of the hMRI Toolbox up-to-date with the repository, and developers who wish
to [contribute](Contribute) to the Toolbox development, it is necessary to have Git installed. You can download
Git [here](https://git-scm.com/downloads). See the [Git documentation pages](https://git-scm.com/doc) to get started if
you are new to Git.

## Install the Toolbox

The installation procedure allows users to manage the hMRI Toolbox repository independently from their SPM installation,
thanks to a [redirection script](#redirection-script). This is
especially convenient when a version control system is used for SPM as well.
The following steps describe how to get your own copy of the Toolbox in a directory of your choice and how to make it
available as an SPM tool.

### Download the latest release

For users of the hMRI Toolbox who do not plan to modify the code by themselves, it is recommended to download the latest
release from the [hMRI Releases](../releases) page. After download, unzip the compressed file and proceed with the
installation instructions below (skip the next section for developers). For information on the various releases and
related changes, please refer to the [Releases wiki page](Releases).

### For developers and advanced users

If you are an advanced user, you might want to clone the hMRI Toolbox repository and keep your version regularly
up-to-date.
The following Git command line will create a hMRI Toolbox directory in the current directory and clone
the toolbox repository within that directory:

```bash
$ git clone https://github.com/hMRI-group/hMRI-toolbox.git
```

If you wish to further develop the toolbox, we recommend you fork the repository.
For more details on forking and more development guidelines, please refer to the [Develop & Contribute](Contribute) wiki
page.

### Redirection script

The redirection script is a short script to be copied to your SPM/toolbox directory. It will point to the full
implementation of the hMRI Toolbox, thus making it available as an SPM tool. The following steps describe how to
proceed:

- Create *(empty)* directory ```hMRI``` into ```<path-to-your-spm>/toolbox```
- Copy ```hMRI-Toolbox/install/tbx_cfg_hmri_redirect.m``` into ```<path-to-your-spm>/toolbox/hMRI```
- Add the hMRI-Toolbox root directory (containing the unzipped release or your local repository including the full
  implementation of the hMRI-Toolbox) to your Matlab Path. NOTE: in Matlab, choose the `Add Folder...` option. Don't use
  the `Add with Subfolders...` option. The appropriate subfolders will be automatically added when starting the toolbox.

### Very first test

- Start Matlab and the SPM (fMRI) user interface: ```spm fmri```,
- Start the Batch GUI (Batch button in the SPM Menu).
- Check whether you can access the hMRI Toolbox: ```SPM > Tools > hMRI Tools```

### Unit tests

A number of unit tests are provided in `hMRI/hmri_code_tests`. To run these you must first ensure that the hMRI Toolbox
is on your Matlab path, then navigate to that folder and run `runtests` in Matlab. Please note that some tests require
data to be downloaded to the `hmri_code_tests` directory. If you have write access, then these data can be downloaded
using `hMRI/hmri_code_tests/hmri_get_ut_data.m` to fix these errors.

## Download dataset & protocol examples

A full dataset is available for [download][hMRI-dataset-zip-to-download], as well as several protocols containing all
acquisition parameters. The following section makes use of this dataset.

## Tutorials

Examples and tutorials are based on the hMRI demo dataset available [here][hMRI-dataset-zip-to-download]. Examples can
also be found in the [`hMRI/examples`]({{config.repo_url}}/blob/master/examples) directory.

#### Available Examples:

- [Map creation step-by-step example](mapCreation#example)
- [Toolbox configuration and customization examples](defaultsAndCustomization#examples)

## Help and resources

This webpage is the main source of information to learn about how to use the toolbox and how it works, together
with the toolbox paper ([pre-print](http://dx.doi.org/10.20347/WIAS.PREPRINT.2527)).

On top of this, help information can be found in the **Help section of each matlab script**
(or by typing `help <function>` in Matlab) and in the **SPM Batch GUI** in the Help box at the bottom of the Batch
window.

If you cannot find the expected information above,
you may want to browse the [mailing list archive](https://www.jiscmail.ac.uk/cgi-bin/webadmin?A0=HMRI-TOOLBOX)
(you must register to the mailing list, get a password and log in as registered user to access the archive)
or the [issue tracking system]({{config.repo_url}}/issues).

If you still cannot find an answer to your question or a solution to your issue, you can either email the
mailing list or open a new issue. Guidelines and practical information on using the mailing list and the issue tracking
system can be found on the [Develop & Contribute](../development/contribute.md) page.

## Note on Runtimes

The runtime of the various processes depends of course on the available hardware.
On a MacBookPro with i7 (2.9GHz) with 16GB RAM and macOS 10.13.6 (High Sierra) with Matlab 2017b
the creation of qMRI maps with the toolbox using the example dataset takes approximately 11 minutes.
This includes the transmit and receive bias correction. The spatial processing of multiple subjects relies
on adjusted SPM functionalities. Thus, the runtime for this part of the toolbox is very similar to the runtime
known for the corresponding processes in SPM.


[hMRI-dataset-zip-to-download]: hmriDemoDataset.md