## What is this?
Using imgur's API to upload random gifs store them in S3, and store the metadata in dynamodb. Designed to run using [zappa](www.zappa.io) (AWS Lambda + API gateway) but can be run as as a normal Flask application.

## Requirements
* AWS account
* imgur api keys - you can get those [here](https://api.imgur.com/oauth2/addclient)
* virtualenv
* [zappa](www.zappa.io) (optional if deploying to lambda)
* Dynamodb table
* S3 bucket

## Setup
Create Dynamodb table with the primary key of `id`.  
Create S3 bucket

Download this project and install requirements to virtualenv
```
git clone
cd imgur-zappa
virtualenv env
source env/bin/activate
pip install -r requirements.txt
```
Assign values for the following in imgur_zappa.py;
```
# imgur api keys
imgur_client_id = ''
imgur_client_secret = ''

dynamodb_table = ''
# s3 bucket to store your gifs
s3_bucket=''
# dynamodb region
region = ''
```
Fire it up!!
```
python imgur_zappa.py
```
## Usage
You can curl the root endpoint and receive JSON back containing links to the image and metadata.
```
$ curl http://127.0.0.1:5000
{
  "metadata": "http://127.0.0.1:5000/metadata/id/fwSiuOw",
  "s3_link": "https://somebucket.s3.amazonaws.com/fwSiuOw.gif"
}
```

Curling the metadata link will return the data from the dynamodb table.
```
$ curl http://127.0.0.1:5000/metadata/id/fwSiuOw
{
  "animated": true,
  "bandwidth": 21192206821100,
  "comment_count": 221,
  "datetime": 1436143033,
  "downs": 141,
  "gifv": "http://i.imgur.com/fwSiuOw.gifv",
  "height": 245,
  "id": "fwSiuOw",
  "in_gallery": true,
  "link": "http://i.imgur.com/fwSiuOw.gif",
  "looping": true,
  "mp4": "http://i.imgur.com/fwSiuOw.mp4",
  "mp4_size": 504606,
  "points": 5409,
  "score": 8751,
  "section": "gifs",
  "size": 4605830,
  "title": "Amazing egg crack by chef ",
  "type": "image/gif",
  "ups": 5550,
  "views": 4601170,
  "width": 250
}
```
## Deploy to Lambda
Using zappa makes this super easy.
```
pip install zappa
zappa init
zappa deploy
```
Welcome to the future!
