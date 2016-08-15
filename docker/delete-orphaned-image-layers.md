Delete Orphaned Docker Image Layers
===================================

The following command will delete any Docker image cruft that's built up over
time - image layers that are no longer referenced by tags:

    docker rmi $(docker images --filter "dangling=true" -q --no-trunc)
