# Luma Video Generation API

Luma AI video generation service creates beautiful videos through prompts and starting images.

API home page: [Ace Data Cloud - Luma Video Generation](https://platform.acedata.cloud/services/dff45a0d-2858-4936-8e2b-3d49c52aff11)

## Get Started


With the widespread application of AI, various AI programs have gradually become popular. AI has gradually penetrated all aspects of people's work and life. The industries involved in AI are also increasing, from the initial writing, to medical education, and now to video.

Luma is a professional high-quality video generation platform where users only need to upload materials to automatically generate high-quality videos based on different styles and effects. This AI video generator is developed by team members from well-known technology companies, aiming to allow everyone to easily create outstanding videos without complex editing tools.

However, Luma does not officially provide an API. AceDataCloud offers a set of Luma APIs that simulate the integration with Suno's official API, making it convenient and quick to generate the desired videos.

### Application and Usage

To use the Luma Videos API, you can first visit the [Luma Videos Generation API](https://platform.acedata.cloud/documents/5bd3597d-1ff8-44ad-a580-b66b48393e7f) page and click the "Acquire" button to obtain the credentials needed for the request:

![](https://cdn.acedata.cloud/nyq0xz.png)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

Upon the first application, there will be a free quota provided, allowing you to use the API for free.

### Basic Usage

To generate a video, you can input any text. For example, if I want to generate a video about astronauts shuttling between space and volcanoes, I can input `Astronauts shuttle from space to volcano`, as shown in the image:

<p><img src="https://cdn.acedata.cloud/yub02j.png" width="500" class="m-auto"></p>

The generated code is as follows:

<p><img src="https://cdn.acedata.cloud/ieq7yn.png" width="500" class="m-auto"></p>

Main request parameters:

- `prompt`: The prompt for generating the video.
- `aspect_ratio`: The aspect ratio of the video, default is 16:9.
- `end_image_url`: Optional, specifies the end frame.
- `enhancement`: Optional, clarity enhancement switch.
- `loop`: Whether to generate a looping video, default is false.
- `timeout`: Optional, timeout in seconds.
- `callback_url`: Asynchronous callback address.

You can click the "Try" button to directly test the API. After waiting for 1-2 minutes, the result is as follows:

```json
{
  "success": true,
  "task_id": "e4018a99-1522-4f24-9330-62c2a9b50b59",
  "video_id": "155838f8-7f1e-44d8-b387-192f3b4b509d",
  "prompt": "Astronauts shuttle from space to volcano",
  "video_url": "https://storage.cdn-luma.com/dream_machine/af94e7ca-da35-4b5f-a636-2d7254184d0d/watermarked_video0585de3737db946e5a0ac895384ecd180.mp4",
  "video_height": 752,
  "video_width": 1360,
  "state": "completed",
  "thumbnail_url": "https://platform.cdn.acedata.cloud/luma/e4018a99-1522-4f24-9330-62c2a9b50b59.jpg",
  "thumbnail_width": 1360,
  "thumbnail_height": 752
}
```

At this point, we have obtained the relevant information about the video, including video ID, video link, video cover, and other content.

Field descriptions are as follows:

- success: Whether the generation was successful; if successful, it is `true`, otherwise it is `false`.
- task_id: The unique ID of this video generation task.
- video_id: The unique ID of the video generated from this task.
- prompt: The keywords for this video generation task.
- video_url: The result video link of this video generation task.
- video_height: The height of the generated video cover image.
- video_width: The width of the generated video cover image.
- state: The status of this video generation task; if the task is completed, it is `completed`.
- thumbnail_url: The link to the generated video cover image.
- thumbnail_width: The width of the generated video cover image.
- thumbnail_height: The height of the generated video cover image.

### Custom Start and End Frame Generation

If you want to generate a video by customizing the start and end frames, you can input the image links for the start and end frames:

At this point, the `start_image_url` field can accept the following image as the start frame of the video:

![Start Frame](https://cdn.acedata.cloud/r9vsv9.png)

Next, we need to customize the video generation based on the start and end frames and keywords, specifying the following content:

- action: The action of the video generation task, usually normal generation `generate` and extended generation `extend`, default is `generate`.
- start_image_url: Specifies the start frame of the generated video.
- end_image_url: Specifies the end frame of the generated video.
- prompt: The keyword content for generating the video.

An example of the input is as follows:

<p><img src="https://cdn.acedata.cloud/zvzydx.png" width="500" class="m-auto"></p>

After filling it out, the generated code is as follows:

<p><img src="https://cdn.acedata.cloud/tx80pu.png" width="500" class="m-auto"></p>

Corresponding code:

```python
import requests

url = "https://api.acedata.cloud/luma/videos"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "start_image_url": "https://cdn.acedata.cloud/r9vsv9.png",
    "action": "generate",
    "prompt": "Astronauts shuttle from space to volcano"
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

The result obtained is as follows:

```json
{
  "success": true,
  "task_id": "12a18694-fd4b-47e7-9c50-34f30862cff6",
  "video_id": "0105c090-03a5-425a-8026-523341cd575b",
  "prompt": "Astronauts shuttle from space to volcano",
  "video_url": "https://platform.cdn.acedata.cloud/luma/12a18694-fd4b-47e7-9c50-34f30862cff6.mp4",
  "video_height": 656,
  "video_width": 1552,
  "state": "completed",
  "thumbnail_url": "https://platform.cdn.acedata.cloud/luma/12a18694-fd4b-47e7-9c50-34f30862cff6.jpg",
  "thumbnail_width": 1552,
  "thumbnail_height": 656
}
```

The final result is similar to the previous one, with the generated video start frame containing the image we provided. Of course, you can also provide both start and end frame image links to generate the video; you just need to add an end frame image based on the above. The information for the end frame image is as follows:

![End Frame](https://cdn.acedata.cloud/0iad3k.png)

An example of the input is as follows:

<p><img src="https://cdn.acedata.cloud/20igwi.png" width="500" class="m-auto"></p>

Finally, the result is as follows:
```json
{
  "success": true,
  "task_id": "d1cb723a-e554-4775-94a4-bb6ae8c7ea67",
  "video_id": "6bebd0d2-f793-472e-9326-38528a9273bb",
  "prompt": "Astronauts shuttle from space to volcano",
  "video_url": "https://platform.cdn.acedata.cloud/luma/d1cb723a-e554-4775-94a4-bb6ae8c7ea67.mp4",
  "video_height": 656,
  "video_width": 1552,
  "state": "completed",
  "thumbnail_url": "https://platform.cdn.acedata.cloud/luma/d1cb723a-e554-4775-94a4-bb6ae8c7ea67.jpg",
  "thumbnail_width": 1552,
  "thumbnail_height": 656
}
```

The result is similar to the above text, and the generated video also contains images of the first and last frames, thus completing the custom first and last frame generation for the video.


## More

For more info, please check below APIs and integration documents.

| API | Path | Integration Guidance |
| ---- | ---- | ------------ |
| [Luma Videos Generation API](http://platform.acedata.cloud/documents/5bd3597d-1ff8-44ad-a580-b66b48393e7f) | `/luma/videos` | [Luma Videos Generation API Integration Guide](http://platform.acedata.cloud/documents/16bb0108-1f09-45b3-97ac-16a668750a8c) |
| [Luma Tasks API](http://platform.acedata.cloud/documents/7d32369c-4ead-4364-a4c5-652bc768b3ff) | `/luma/tasks` | [Luma Tasks API Integration Guide](http://platform.acedata.cloud/documents/3e965973-7b28-4844-b404-bd28bfeb5097) |

Base URL: [https://api.acedata.cloud](https://api.acedata.cloud)

## Support

If you meet any issue, check our from [support info](https://platform.acedata.cloud/support).