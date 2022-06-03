Public documentation: https://everyonesocial.github.io/everyonesocial-api/
# Getting access


## API Token
Currently the API is in beta, and access can be granted by your account team.

Access to the API Token will be available through your account settings once it is live. Example URL: https://customer_name.everyonesocial.app/account

## OAuth

Currently not implemented, but it will come soon.

# Steps to create a post

The following steps are how to create an Image post in Python.

# Creating the initial content.

For this example there are the variables that you will need. You will need to switch out the `api_key` and the `group_id` 
## Initial Data
```python
import requests
import json

api_key = '3be0ed19-83ee-4fae-8ff8-69bfab4f5070'
group_id = '0881d766-6bc2-4879-a4ec-b9861c704114'
url = f'https://api.everyonesocial.app/v1/group/{group_id}/content'

headers = {'Authorization': api_key}
```

## Create the initial post
The following will initialize the post creation, providing the content. 
```python
create_post_payload = {
    'title': 'brian_test',
    'body': 'pycharm test'
}
result = requests.post(url, json=create_post_payload, headers=headers)
result.raise_for_status()
content_result = result.json()
```

## Optional - Request to attach an image.

If you are creating a post with an image, the following call will return the URL of where to upload your image to.

```python
result = requests.post(url + f"/{content_result['id']}/image", headers=headers)
result.raise_for_status()
image_post_info = result.json()
```

## Optional - Uploading the image

Assuming you made the call above, the response data can be found in the `image_post_info`. Using that, the following will upload your image.
```python
with open('/path/to/your/image.jpg', 'rb') as file_obj:
    http_repsonse = requests.post(image_post_info['url'], data=json.loads(image_post_info['post_data']), files={'file': file_obj})
    http_repsonse.raise_for_status()
```

## Publish the content

Finally, post the content to go live. This will follow the policy of the group, either immediate go live or wait for moderator approval.

```python
result = requests.post(url + f"/{content_result['id']}/publish", json=publish_payload, headers=headers)
result.raise_for_status()
```
