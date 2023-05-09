I created this "animal image generator" because I wanted to update my github profile readme and I wanted to learn more about Github Action Workflows and Bash scripting. Why not ~~generate 2 birds with one stone?~~ do both at the same time :)

The way it works is that every single time an issue is created with the prefix title "newAnimalImage|" the github workflow would run. The first step is to checkout the repo in order to access the README.md file.

Next was fetching the random animal image URL using the Unsplash API:

```
animal_image_url=$(curl -s "https://api.unsplash.com/photos/random?query=animals&orientation=landscape" -H "Authorization: Client-ID ${{ secrets.UNSPLASH_CLIENT_ID }}" | jq -r '.urls.regular')
```

This step generates a new animal picture URL by making a request to the Unsplash API using the curl command. The response is then parsed using jq to extract the URL of the image, which is stored in the animal_image_url variable.

```
sed -i '0,/<img/{/<img/d;}' README.md
echo "<img src=\"$animal_image_url\">" >> README.md
```

The sed command removes any existing <img> tags from the README.md file, and the new image URL is added to the file using the echo command.

From there the workflow commits the changes to the README.md with a newly updated image. It then pushes the commits onto the main branch, as well as closes the issue that initiated the workflow.
