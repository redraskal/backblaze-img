# backblaze-img
:fire: File uploading service utilizing Backblaze B2 Cloud Storage and Fastify.

## Installation

1. Clone the branch

   ```
   git clone https://github.com/JediMasterSoda/backblaze-img.git
   ```

2. Install dependencies

   ```
   yarn install
   ```

    or alternatively

   ```
   npm install
   ```

3. Rename config.json.example to config.json and edit config.json along with [backblaze-img.sxcu](#backblaze-imgsxcu). More on that at [Configuration](#configuration).

4. Run 

   ```
   yarn start
   ```

    or 

   ```
   npm start
   ```

5. Install [ShareX](https://getsharex.com/) and open the [backblaze-img.sxcu](#backblaze-imgsxcu) settings file.

## Configuration

### Fastify

| hostname | “localhost” for testing and your server’s IP address or “0.0.0.0” for production. |
| logger   | Only enable for debugging because Fastify runs slower with logging enabled. |

### General

| fileUrl         | URL you will receive in ShareX to view the uploaded file. {0} is replaced with a file ID generated by backblaze-img. There are no file extensions with this uploading service. |
| redirect        | URL users will be redirected to when trying to view the index. |
| uploadKeys      | Array of Strings used to authenticate uploaders. Change the default key “2d9f4679-9a28-4fdc-866e-06c2b1fc1b39” to something unique and extremely random. You will need this key for [backblaze-img.sxcu](#backblaze-imgsxcu). |
| uploadLimit     | The upload limit for any given file.                         |
| fileCacheMaxAge | The Cache-Control header max age for any given file.         |
| errorCacheTTL   | How long in milliseconds file 404’s should be stored in memory. This feature is useful to reduce the amount of requests on Backblaze since they charge per request after a certain threshold. |

### b2

1. [Create a Backblaze account](https://www.backblaze.com/b2/sign-up.html).
2. [Create a bucket](https://secure.backblaze.com/b2_buckets.htm). Your bucket unique name can be anything, just make sure to set the files to “Private” mode so random people on the Internet won’t be able to view all of your files.
3. Copy the Bucket ID you will see on the same page into your config.json along with the bucket name you chose.
4. Go to https://secure.backblaze.com/app_keys.htm and “Add a New Application Key”. Name the key, set the allow access to just the bucket you created and not “All”, set the “Type of Access” to “Read and Write” and hit “Create New Key”. You will see a “keyID” and a “applicationKey”. Copy those two values to your config.json as well.

| keyID          | See above steps.                                             |
| applicationKey | See above steps.                                             |
| bucketName     | See above steps.                                             |
| bucketId       | See above steps.                                             |
| authCacheTTL   | Backblaze generates an authorization token for API usage in this project. The token will be revoked after 24 hours so it must be refreshed at the correct time in order to avoid website errors. This number is the time in milliseconds between token refreshes. The value is currently set at 1 hour. |

### backblaze-img.sxcu

1. Modify {WEBSITE_URL} to the URL this project is hosted at.
2. Change {UPLOAD_KEY} to the upload key you modified in [config.json](#Configuration).

```json
{
  "Version": "13.1.0",
  "Name": "backblaze-img",
  "DestinationType": "ImageUploader, FileUploader",
  "RequestMethod": "POST",
  "RequestURL": "https://{WEBSITE_URL}/upload/{UPLOAD_KEY}",
  "Body": "Binary",
  "URL": "$json:file_url$"
}
```
