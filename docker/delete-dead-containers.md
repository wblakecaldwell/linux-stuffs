Delete all Docker containers with status of 'Dead'
==================================================

Pick which approach works for you:

1. Simple one-liner that will barf with an error message and exit 
   status of '1' if there aren't any:

        docker rm $(docker ps -a | cut -c1-12,89-93 | grep 'Dead' | cut -c1-12)

2. Same one-liner that still barfs, but will always exit with '0'

        docker rm $(docker ps -a | cut -c1-12,89-93 | grep 'Dead' | cut -c1-12) || :

3. Check for dead containers before deleting them 

        if [ ! -z $(docker ps -a | cut -c1-12,89-93 | grep 'Dead') ]; then 
          docker rm $(docker ps -a | cut -c1-12,89-93 | grep 'Dead' | cut -c1-12)
        fi

4. Check for dead containers before deleting them, and print what's going on

        DEAD_CONTAINERS=$(docker ps -a | cut -c1-12,89-92 | grep 'Dead')
        if [ ! -z "${DEAD_CONTAINERS}" ]; then
          DEAD_CONTAINER_COUNT=$(echo $DEAD_CONTAINERS | wc -w)
          echo "Deleting ${DEAD_CONTAINER_COUNT} dead Docker containers"
          docker rm $(echo $DEAD_CONTAINERS | sed -e 's/Dead/ /g')
        fi

