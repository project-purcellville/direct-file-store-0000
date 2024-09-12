# Direct File Store for `angr`

A collection of binaries to be used in the `angr` decompilation corpus test suite.

**Current state of binary corpus**

| Component                                       | Count | Max file size | Total file size |
| ----------------------------------------------- | ----- | ------------- | --------------- |
| CGC Challenge binaries - Linux 32-bit           | 570   | 30,859,012    | 188,323,600     |
| CGC Challenge binaries - Linux 64-bit           | 570   | 23,189,760    | 196,931,592     |
| CGC Challenge binaries - Windows 64-bit (clang) | 562   | 17,622,016    | 116,242,432     |
| **Total**                                       | 1702  |               | 501,501,720     |

## Organization

The binaries in this repository are organized by directory based on build version and source.
This structure is crucial for the corpus test suite in the `angr` pipeline, which runs smaller
subset tests based on the directory structure. If any major directory structure changes occur
in this repository, please update the GitHub Action variables in the `angr` pipeline accordingly.

### Top level directories

Currently there are two top-level directories: `cgc-challenges` and `stable`.
`cgc-challenges` holds the full corpus of binaries summarized in the table
above. `stable` holds a smaller subset that were deemed to be more stable,
in the sense that the runs of the `angr` decompiler were more likely to
produce consistent results. A subset of `cgc-challengers` is used in pull
request-triggered actions, whereas the stable binaries are used for the
nightly decompiler corpus test.

### Actions related to this repo

#### Add a file

To add files to the corpus, contributors will create a branch and corresponding pull request.
They will add files to a designated folder in the repository following the conventions above.

**NOTE**: _Make sure the table above in this README file is updated with the new file information on_
_file additions_

#### Delete a file

To remove files from the corpus, contributors will create a branch and corresponding pull request. Contributors
will include a reason for selecting the binaries for deletion. If a particular file must be purged from the git
history, then follow the traditional method for removing files from git history (with caution).
https://git-scm.com/docs/git-filter-branch or their recommended https://github.com/newren/git-filter-repo/

**NOTE**: _Make sure the table above in this README file is updated with the new file information on_
_file deletions_

#### Retrieve Corpus Binary File

Code examples are provided below on how to download files from the GitHub release with curl, Python, and the GitHub CLI (gh).

##### Using curl directly with raw.githubusercontent:

```shell
$ file=cgc-challenges/linux-build/challenges/3D_Image_Toolkit/3D_Image_Toolkit
$ curl https://raw.githubusercontent.com/project-purcellville/direct-file-store-0000/main/$file --output out_file

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 73788  100 73788    0     0   947k      0 --:--:-- --:--:-- --:--:--  960k
```

##### Using a python library

```python
import requests

file = "cgc-challenges/linux-build/challenges/3D_Image_Toolkit/3D_Image_Toolkit"
url = f"https://raw.githubusercontent.com/project-purcellville/direct-file-store-0000/main/{file}"
response = requests.get(url)
with open("out_file", "wb") as f:
    f.write(response.content)
```

##### Using the gh CLI

```shell
gh api repos/project-purcellville/direct-file-store-0000/contents/cgc-challenges/linux-build/challenges/3D_Image_Toolkit/3D_Image_Toolkit --jq '.download_url' | xargs curl -L -o out_file

```
