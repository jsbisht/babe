## Recursively rename files within a directory

Rename _.js files to _.ts

```
find . -depth -exec rename 's/.js$/.ts/' {} +
```

## For loop on array of items

```sh
for collection in 'videos' 'comedians' 'channelIds' 'latest' 'lengthy' 'multistarrer' 'popular' 'trending'
do
  firebase firestore:delete /$1-$collection-0 --recursive --yes
  echo "Deleted $collection from firebase"
done
```

## Search a file using file name

```sh
find . -name *.html
```

## Search and replace text in file

```sh
sed -i 's/<search-text>/<replace-text>/g' file.txt
```

Use double quotes to use variable:

```sh
sed -i "s/mistake/update/g" file.txt
```

## Search text in a folder

```sh
grep -rnw ./client/*.json -e "deploy"
```

## Curl

### Add --trace - to view body

View body of the curl request using:

```
curl --trace - \
--location --request PUT 'http://localhost:8585/coco-india/asia-south1/api/use/active-db/set' \
--header 'Content-Type: application/json' \
--data-raw '{"id": '${newActiveDbId}'}'
```
