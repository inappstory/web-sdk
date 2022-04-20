<p align="center"><a href="https://inappstory.com" target="_blank" rel="noopener noreferrer"><img width="200" src="https://inappstory.com/images/logo.png" alt="InAppStory logo"></a></p>


# Story share page

## Introduction

This guide explains how to create a page that handles `https://domain.com/s/{storyId}`

URL template is configured in the [project settings](https://console.inappstory.com)

## Get data for page layout

API endpoint: `GET https://api.inappstory.com/v2/story-share-page-info/{storyId}`

Bearer token is used for authentication - it is passed in the Authorization header: `Authorization: Bearer {token}`,
where token - project integration key

Possible HTTP response codes:

* 403 - not transferred or incorrect Bearer token or the project is not active
* 404 - The requested story was not found
* 200 - ok

The response body is a json object with the following fields:

### Story share page data

| Variable | Type | Description |
|----------|------|-------------|
| title       | string | Story title |
| description | string | Story description |
| image       | object &#124; null | [Image](#story-share-page-image) |
| story_data  | object | Internal data for Stories widget |

### Story share page image

| Variable | Type | Description |
|----------|------|-------------|
| url | string | Image url |
| width | number | Image width, `px` |
| height | number | Image height, `px` |

## Page layout

```html
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
    <title>{title}</title>
    <meta name="title" content="{title}">
    <meta name="description" content="{description}">
    <meta property="og:type" content="website">
    <meta property="og:url" content="{url}">
    <meta property="og:title" content="{title}">
    <meta property="og:description" content="{description}">
    <meta property="og:image" content="{image_url}">
    <meta property="og:image:width" content="{image_width}">
    <meta property="og:image:height" content="{image_height}">
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="{title}">
    <meta name="twitter:description" content="{description}">
    <meta name="twitter:url" content="{url}">
    <meta name="twitter:site" content="{twitter_username}">
    <meta name="twitter:image" content="{image_url}">
    <meta property="og:site_name" content="{site_name}">

    <!-- image_url, image_width, image_height - one of images from API response -->
    <!-- url - canonical page url -->
    <!-- title, description - from API response -->
    <!-- twitter_username, site_name - if need it -->
</head>
<body>

<script>
  // This code loads the web-sdk API code asynchronously 
  // and create queue in global var window.IASReady.

  window.IASReady = (function (d, s, id) {
    var js, fjs = d.getElementsByTagName(s)[0], st = window.IASReady || {};
    if (d.getElementById(id)) return st;
    js = d.createElement(s);
    js.id = id;
    js.src = "https://sdk.inappstory.com/v2.1.0/dist/js/IAS.js";
    js.async = true;
    fjs.parentNode.insertBefore(js, fjs);
    st._e = [];
    st.ready = function (f) {
      st._e.push(f);
    };
    return st;
  }(document, "script", "ias-wjs"));

  // 3. This function creates an StoryManager instance (and SharePage widget)
  //    after the API code downloads.
  window.IASReady.ready(function () {

    const storyManagerConfig = {
      apiKey: "{project-integration-key}",
      userId: "{user-id-hash}", // usually - hash from real user identifier
      placeholders: {
        user: "Guest"
      },
      lang: "ru"
    };

    // StoryManager singleton instance
    const storyManager = new window.IAS.StoryManager(storyManagerConfig);

    // AppearanceManager instance
    const appearanceManager = new window.IAS.AppearanceManager();

    // appearance config
    appearanceManager.setCommonOptions({
      hasLike: true,
      hasFavorite: true
    })
      .setStoryReaderOptions({
        closeButtonPosition: 'right',
        scrollStyle: 'flat',
      });

    // Show SharePage
    const id = "krtdhze"; // from url path
    const sharePageOptions = {
      handleStartLoad: () => console.log("handleStartLoad"),
      handleStopLoad: (result) => console.log("handleStartLoad", result),  // result: boolean - were onboarding or not
      handleStoryReaderClose: () => console.log("handleStoryReaderClose")
    };
    const sharePage = new storyManager.SharePage(id, appearanceManager, sharePageOptions);

    // or events can handle over eventEmitter
    // sharePage.on("startLoad", () => console.log("startLoad"));
    // sharePage.on("endLoad", (e) => console.log("endLoad", e.result));
    // sharePage.on("closeStoryReader", () => console.log("closeStoryReader"));

    // clicks on buttons in reader
    storyManager.storyLinkClickHandler = payload => console.log({payload});


  });
</script>
</body>
</html>
```

## Examples

<details>
  <summary>Nuxt.js</summary>

```vue

<template>
</template>
<script lang="js">
import Vue from "vue";
import isEmpty from 'lodash/isEmpty'
import isArray from 'lodash/isArray'
import axios from 'axios';

export default Vue.extend({
    layout: false,
    head() {
        return {
            title: this.title,
            description: this.description,
            script: this.script,
            meta: this.meta
        }
    },
    mounted() {
        if (process.client) {
            window.addEventListener("load", () => {

              const storyManagerConfig = {
                apiKey: this.$config.storiesSdkKey,
                userId: "{user-id-hash}", // usually - hash from real user identifier
                placeholders: {
                  user: "Guest"
                },
                lang: "ru"
              };

              // StoryManager singleton instance
              const storyManager = new window.IAS.StoryManager(storyManagerConfig);

              // AppearanceManager instance
              const appearanceManager = new window.IAS.AppearanceManager();

              // appearance config
              appearanceManager.setCommonOptions({
                hasLike: true,
                hasFavorite: true
              })
                .setStoryReaderOptions({
                  closeButtonPosition: 'right',
                  scrollStyle: 'flat',
                });

              // Show SharePage
              const sharePageOptions = {
                handleStartLoading: () => console.log("handleStartLoading"),
                handleStopLoading: (result) => console.log("handleStartLoading", result),  // result: boolean - were onboarding or not
                handleStoryReaderClose: () => console.log("handleStoryReaderClose")
              };
              const sharePage = new storyManager.SharePage(this.$route.params.id, appearanceManager, sharePageOptions);

              // clicks on buttons in reader
              storyManager.storyLinkClickHandler = payload => console.log({payload});
              
            });
            
        }
    },
    validate({params}) {
        return !(params.id === undefined || isEmpty(params.id));
    },
    async asyncData({$config, params, error, req}) {
        const id = params.id
        if (id === undefined) {
            return error({statusCode: 404, message: 'Page not found'})
        }

        let data;
        try {
            const response = await axios.get($config.storiesApiBaseDomain + '/v2/story-share-page-info/' + id, {
                headers: {
                    Authorization: 'Bearer ' + $config.storiesSdkKey
                },
            });
            data = response.data;
        } catch (err) {
            if (err.response) {
                const {status} = err.response
                if (status === 404) {
                    return error({statusCode: 404, message: 'Page not found'})
                }
            }
            return error(err)
        }

        const title = data.title
        const description = data.description
        const image = data.image

        const url = req?.headers.host;

        let imageUrl = null
        let imageWidth = null
        let imageHeight = null

        if (image !== null) {
              imageUrl = image.url
              imageWidth = image.width
              imageHeight = image.height
        }

        const script = [
            {
                src: $config.storiesSdkBaseDomain + '/v2.1.0/dist/js/IAS.js'
            }
        ];

        let meta = [
            {property: 'og:type', content: 'website'},
            {property: 'og:url', content: url},
            {property: 'og:title', content: title},
            {property: 'og:description', content: description},

            {property: 'twitter:card', content: 'summary_large_image'},
            {property: 'twitter:title', content: title},
            {property: 'twitter:url', content: url},
        ];

        if (imageUrl !== null) {
            meta.push({property: 'og:image', content: imageUrl});
            meta.push({property: 'og:image:width', content: imageWidth});
            meta.push({property: 'og:image:height', content: imageHeight});

            meta.push({property: 'twitter:image', content: imageUrl});
        }

        return {
            title, description, imageUrl, imageWidth, imageHeight, url, script, meta
        }

    }
})
</script>
```
</details>