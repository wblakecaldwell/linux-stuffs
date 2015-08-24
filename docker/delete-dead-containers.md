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

