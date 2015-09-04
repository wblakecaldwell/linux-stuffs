Delete all Docker containers with status of 'Dead'
==================================================

Here's the long way, if you're using a version of Docker that doesn't have the `--format` option:

      # Find the start of the 'STATUS' column
      COL_START=`docker ps -a | head -n1 | grep -aob 'STATUS' | grep -o '[0-9]*'`
      
      # We're looking for 'Dead', which is... 4 characters
      COL_END=$(( $COL + 4 ))
      
      # Find all all of the containers IDs where the status is 'Dead'
      DEAD_CONTAINERS=$(docker ps -a | cut -c1-12,"${COL_START}-${COL_END}" | grep 'Dead' | cut -c1-12 )
      if [ ! -z "${DEAD_CONTAINERS}" ]; then
        # Report what we're doing
        DEAD_CONTAINER_COUNT=$(echo $DEAD_CONTAINERS | wc -w)
        echo "Deleting ${DEAD_CONTAINER_COUNT} dead Docker containers"
      
        # Delete the dead containers
        docker rm $(echo $DEAD_CONTAINERS | sed -e 's/Dead/ /g')
      fi
