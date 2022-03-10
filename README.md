# Drift Dockerfiles

This repository contains Dockerfiles for each of the drift test cases we expect to confirm.

**OS Packages**

- Alpine
- Debian
- RHEL (UBI)
- Ubuntu
- Unknown

**Language Packages**

- Java
- NPM
- Python
- Go
- Ruby
- NuGet

**Testing**

For each of the above, the following steps can be performed to prepare the images for the environment.

**Anchore Prep**

1. Create a new policy bundle for drift testing
2. Create a three policy rules which use the `tag_drift` gate and each different trigger (e.g. packages added, packages removed, packages modified).
3. Activate this policy bundle
4. This will evaluate each of the three cases when performing the drift comparison. 

**Image Prep**

**E.g. Alpine**

1. Navigate to the alpine directory and build an image from the Dockerfile. 
2. Tag the image (e.g. `dev`) and push to a repository. E.g. `docker.io/jvalance/alpine-drift-test:dev`
3. Build a second image from the other Dockerfile in the directory (e.g. `Dockerfile2`).
4. Tag the image with the same tag as the first image (e.g. `dev`) and push to the same repository. E.g. `docker.io/jvalance/alpine-drift-test:dev`
5. You should now have two distinct digests associated with the same `registry/repository:tag`. Anchore will compare these two digests when determining the content changes (e.g. packages added, packages removed, packages modified).
6. In this example, we expect packages to be added, as the second Dockerfile describes an additional package be installed (e.g. nginx).


