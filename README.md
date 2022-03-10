# Test Dockerfiles

This repository contains Dockerfiles for each of the test cases we expect to confirm.

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

**Anchore Prep**

1. Create a new policy bundle for drift testing
2. Create a three policy rules which use the `tag drift` gate and each different trigger (e.g. packages added, packages removed, packages modified).
3. Activate this policy bundle
4. This will evaluate each of the three cases when performing the drift comparison. 

For each of the above OS and language packages, the following steps can be performed to prepare the images for the environment.

**Image Prep**

**E.g. Alpine**

1. Navigate to the alpine directory and build an image from the Dockerfile. 
2. Tag the image (e.g. `dev`) and push to a repository. E.g. `docker.io/jvalance/alpine-drift-test:dev`
3. Add the first image to Anchore. E.g. `anchore-cli image add docker.io/jvalance/alpine-drift-test:dev`
4. Build a second image from the other Dockerfile in the directory (e.g. `Dockerfile2`). You can add additional packages to the Dockerfile if you like. 
5. Tag the image with the same tag as the first image (e.g. `dev`) and push to the same repository. E.g. `docker.io/jvalance/alpine-drift-test:dev`
6. You should now have two distinct digests associated with the same `registry/repository:tag`. Anchore will compare these two digests when determining the content changes (e.g. packages added, packages removed, packages modified).
7. Add the second image to Anchore. E.g. `anchore-cli image add docker.io/jvalance/alpine-drift-test:dev`
8. In this example, we expect packages to be added, as the second Dockerfile describes an additional package be installed (e.g. nginx).
9. Create a third image, add or remove some packages, build and push to a repository. Should now have three digests.
10. Add the third image to Anchore. E.g. `anchore-cli image add docker.io/jvalance/alpine-drift-test:dev`


