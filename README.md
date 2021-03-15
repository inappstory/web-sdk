<p align="center"><a href="https://inappstory.com" target="_blank" rel="noopener noreferrer"><img width="200" src="https://inappstory.com/images/logo.png" alt="InAppStory logo"></a></p>

# Stories Widget

## Introduction

Web-sdk API lets you embed a Stories` widget on your website and control it using JavaScript.

## Getting started

```html
<!DOCTYPE html>
<html>
<body>
<!-- 1. The <iframe> (and Stories` widget) will be mounted to this <div> tag -->
<div id="stories_widget"></div>

<script>
    // 2. This code loads the web-sdk API code asynchronously 
    // and create queue in global var window.IASReady.

    window.IASReady = (function (d, s, id) {
        var js, fjs = d.getElementsByTagName(s)[0], st = window.IASReady || {};
        if (d.getElementById(id)) return st;
        js = d.createElement(s);
        js.id = id;
        js.src = "https://sdk.inappstory.com/js/iframe_api.js";
        js.async = true;
        fjs.parentNode.insertBefore(js, fjs);
        st._e = [];
        st.ready = function (f) {
            st._e.push(f);
        };
        return st;
    }(document, "script", "ias-wjs"));

    // 3. This function creates an <iframe> (and Stories` widget)
    //    after the API code downloads.
    window.IASReady.ready(function () {
        var stories = new window.IAS.Stories("stories_widget", {
            apiKey: "test-key",
            userId: "123",
            tags: "moscow,travels",
            hasLike: true,
            slider: {
                title: {
                    content: 'The best stories',
                    color: '#000',
                    font: 'normal',
                    marginBottom: 20,
                },
                card: {
                    title: {
                        color: 'black',
                        font: '14px/16px InternalPrimaryFont',
                        padding: 8
                    },
                    gap: 10,
                    height: 100,
                    variant: 'quad',
                    border: {
                        radius: 20,
                        color: 'blue',
                        width: 2,
                        gap: 3,
                    },
                    boxShadow: null,
                    opacity: 1,
                    mask: {
                        color: 'rgba(34, 34, 34, 0.3)'
                    },
                    read: {
                        border: {
                            radius: null,
                            color: 'green',
                            width: null,
                            gap: null,
                        },
                        boxShadow: null,
                        opacity: null,
                        mask: {
                            color: 'rgba(34, 34, 34, 0.1)'
                        },
                    },
                },
                layout: {
                    height: 0,
                    backgroundColor: 'transparent'
                },
                sidePadding: 20,
                topPadding: 20,
                bottomPadding: 20,
                bottomMargin: 17,
                navigation: {
                    showControls: false,
                    controlsSize: 48,
                    controlsBackgroundColor: 'white',
                    controlsColor: 'black'
                },
            },
            reader: {
                closeButtonPosition: 'right',
                scrollStyle: 'flat',
            },
            placeholders: {
                user: 'Guest'
            }
        });

        // 4. Override default loading animation
        stories.on('startLoader', function (widget) {
            widget.style.background = 'url("stories/loader.gif") center / 45px auto no-repeat transparent';
        });
        stories.on('endLoader', function (widget) {
            widget.style.background = 'none';
        });

    });
</script>
</body>
</html>
```

1. The `<div>` tag in this section identifies the location on the page where the web-sdk API will place the Stories
   widget. The constructor for the widget object, which is described in the Loading a Stories widget section, identifies
   the `<div>` tag by its id to ensure that the API places the `<iframe>` in the proper location.

2. The code in this section loads the web-sdk API JavaScript code. The example uses DOM modification to download the API
   code to ensure that the code is retrieved asynchronously. (The `<script>` tag's async attribute, which also enables
   asynchronous downloads, is not yet supported in all modern browsers as discussed in
   this [Stack Overflow answer](https://stackoverflow.com/a/1834129).

3. this function creates a widget constructor and adds it to the queue waiting for the API to load

4. The code in this section listens for the start and end events of the story feed widget. When loading starts - adds
   animation and removes when finished.

--- 

## Widget options

| Variable | Type | Description |
|----------|------|-------------|
| apiKey       | string                           | Your project integration key |
| userId       | string &#124; number &#124; null | User id |
| tags         | string  | Tags, separated by commas |
| hasLike      | boolean | Enable "Like" functional. Default disabled |
| slider       | object  | [Slider options](#slider-options) |
| reader       | object  | [Story reader options](#story-reader-options) |
| placeholders | object  | Dict for replace placeholders inside story content or title. Example: {user: "Guest"} |

### Slider options

| Variable | Type | Description |
|----------|------|-------------|
| title          | object | [Slider title options](#slider-title-options) |
| card           | object | [Slider card item options](#slider-card-options) |
| layout         | object | [Slider layout options](#slider-layout-options) |
| sidePadding    | number | Slider side padding, `px`. Default 20 |
| topPadding     | number | Slider top padding, `px`. Default 20 |
| bottomPadding  | number | Slider bottom padding, `px`. Default 20 |
| bottomMargin   | number | Slider bottom margin, `px`. Default 17 |
| navigation     | object | [Slider navigation options](#slider-navigation-options) |

### Slider title options

| Variable | Type | Description |
|----------|------|-------------|
| content         | string &#124; null | Title text. Default null. Title block hidden when value is empty |
| color           | string | CSS valid color value. Default `#ffffff` |
| marginBottom    | number | Title block bottom margin, `px`. Default 20 |
| font            | string | CSS valid font [value](https://developer.mozilla.org/en-US/docs/Web/CSS/font). Override font. <br/>Default `bold 20px/20px InternalPrimaryFont` where InternalPrimaryFont - primary font, loaded in [project settings](https://console.inappstory.com). | 

### Slider layout options

| Variable | Type | Description |
|----------|------|-------------|
| height          | number &#124; null | Slider total height, `px`. `0` - for auto height. Default `0` |
| backgroundColor | string | Default `transparent` |

### Slider card options

| Variable | Type | Description |
|----------|------|-------------|
| title         | object | See below |
| title.color   | string | CSS valid color value. Default `#ffffff` |
| title.padding | number &#124; string | Number, `px` eq for all sides. <br/>String - valid css, for customizing each side. Default `15` |
| title.font    | string | CSS valid font [value](https://developer.mozilla.org/en-US/docs/Web/CSS/font). Override font. <br/>Default `normal 1rem InternalPrimaryFont` where InternalPrimaryFont - primary font, loaded in [project settings](https://console.inappstory.com). | 
| gap           | number | Space between cards, `px`. Default `10` |
| height        | number | Card height, `px`. Default `70` |
| variant       | string | Card style, one of `circle`, `quad`, `rectangle`. Default `circle` |
| border        | object | See below |
| border.radius | number | Card border radius, `px`. Default `0` |
| border.color  | string | Card border color, valid css. Default `black` |
| border.width  | number | Card border width, `px`. Default `2` |
| border.gap    | number | Space between card and border, `px`. Default `3` |
| boxShadow     | string &#124; null | Card box-shadow, valid css value. Default `null` |
| opacity       | number | Card opacity. Default `null` |
| mask          | object &#124; null | Card mask - CSS valid color. Example - `rgba(0,0,0,.3)`. Default `null` |
| read          | object &#124; null | Contain keys: `border`, `boxShadow`, `opacity`, `mask` <br />Apply this values (if current value not null) on card in `read` state. Default all values null |

### Slider navigation options

By default, controls are round buttons with arrow icons at the edges of the slider

| Variable | Type | Description |
|----------|------|-------------|
| showControls            | boolean | Enable slider controls. Default `false` |
| controlsSize            | number |  Button size, `px`. Default `48` |
| controlsBackgroundColor | string | CSS valid color value. Default `#ffffff` |
| controlsColor           | string | CSS valid color value. Default `#000000` |

### Story reader options

| Variable | Type | Description |
|----------|------|-------------|
| closeButtonPosition        | string | Close button position, one of `left`, `right` |
| scrollStyle                | string | Stories viewPager scroll style, one of `flat`, `cover`, `cube` |
| loader.default.color       | string | Default loader primary color. Valid css color |
| loader.default.accentColor | string | Default loader accent color. Valid css color |

---

# Story share page

## Introduction

This guide explains how to create a page that handles `https://domain.com/s/{storyId}`

URL template is configured in the [project settings](https://console.inappstory.com)

## Story data request

API endpoint: `GET https://api.inappstory.com/v2/story-share-page/{storyId}`

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
| title      | string | Story title |
| image      | array | Array of [Image](#story-share-page-image) |
| story_data | object | Internal data for Stories widget |

### Story share page image

| Variable | Type | Description |
|----------|------|-------------|
| url | string | Image url |
| width | number | Image width, `px` |
| height | number | Image height, `px` |
| type | string | Image quality, one of `m`, `h` m - medium, h - height |

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

    <!-- 1. Story widget resources -->
    <link href="https://sdk.inappstory.com/css/sharePage.css" rel="stylesheet">
    <script src="https://sdk.inappstory.com/js/sharePage.js"></script>
</head>
<body>
<!-- 2. <div> for Story widget mount -->
<div id="story_mount"></div>

<!--  3. Init widget with story_data from API -->
<script>
    window.addEventListener("load", function () {
        window.WStories.init(story_data);
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
    <div id="story_mount"></div>
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
            link: this.link,
            script: this.script,
            meta: this.meta
        }
    },
    mounted() {
        if (process.client) {
            window.addEventListener("load", _ => window.WStories.init(this.story));
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
            const response = await axios.get($config.storiesApiBaseDomain + '/v2/story-share-page/' + id, {
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

        const story = data.story_data

        const title = data.title
        const description = data.description
        const image = data.image

        const url = req?.headers.host;

        let imageUrl = null
        let imageWidth = null
        let imageHeight = null

        if (image !== null && image !== undefined && isArray(image)) {
            for (let index in image) {
                if (image.hasOwnProperty(index) && image[index].type === 'h') {
                    imageUrl = image[index].url
                    imageWidth = image[index].width
                    imageHeight = image[index].height
                    break
                }
            }
        }

        const script = [
            {
                src: $config.storiesSdkBaseDomain + '/js/sharePage.js'
            }
        ];

        const link = [
            {
                rel: 'stylesheet',
                href: $config.storiesSdkBaseDomain + '/css/sharePage.css'
            },
            {
                rel: 'canonical',
                href: url
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
            title, description, imageUrl, imageWidth, imageHeight, url, script, link, meta, story
        }

    }
})
</script>
```
</details>













