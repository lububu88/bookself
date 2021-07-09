# OWNERS

Each directory that contains a unit of independent code or content may also contain an OWNERS file. This file applies to everything within the directory, including the OWNERS file itself, sibling files, and child directories. OWNERS files are in YAML format

All users are expected to be assignable. In Gitlab terms, this means they must be members of the organization to which the repo belongs.

### Set up OWNERS
1/ A typical OWNERS and OWNERS_ALIAS (optional) files looks like:
```
# OWNERS
approvers:
- team sre
- team pm, leader project
```


Every repo must have a OWNERS file at the root dir. You can configure each subfolders to have OWNERS files as well. It shoulld have the format like this.
```
repo/
OWNERS
|__sub-dir-01/
|    |__OWNERS
|
|__sub-dir-02/
     |__OWNERS
```

# OWNERS SUBFOLDERs
approvers:
- dev 1
- dev 2
- team pm, leader project
```