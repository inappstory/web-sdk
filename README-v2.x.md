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
    js.src = "https://sdk.inappstory.com/v2.0.0/dist/js/storyManager.js";
    js.async = true;
    fjs.parentNode.insertBefore(js, fjs);
    st._e = [];
    st.ready = function (f) {
      st._e.push(f);
    };
    return st;
  }(document, "script", "ias-wjs"));

  // 3. This function creates an StoryManager instance (and StoriesList widget)
  //    after the API code downloads.
  window.IASReady.ready(function () {

    const storyManagerConfig = {
      apiKey: "{project-integration-key}",
      userId: "kdijhud4454d", // usually - hash from real user identifier
      tags: [], // Array<string>
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
      .setStoriesListOptions({
        title: {
          content: 'The best stories',
          color: '#000',
          font: 'normal',
          marginBottom: 20,
        },
        card: {
          title: {
            color: 'black',
            font: '14px/16px "Segoe UI Semibold"',
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
          opened: {
            border: {
              radius: null,
              color: 'red',
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
        // favoriteCard: {},
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
      })
      .setStoryReaderOptions({
        closeButtonPosition: 'right',
        scrollStyle: 'flat',
      }).setStoryFavoriteReaderOptions({

    });

    // mount and start StoriesList widget
    // #stories_widget - html element selectors
    const storiesList = new storyManager.StoriesList("#stories_widget", appearanceManager);

    // 4. Override default loading animation
    storiesList.on('startLoader', loaderContainer => loaderContainer.style.background = 'url("https://inappstory.com/stories/loader.gif") center / 45px auto no-repeat transparent');
    storiesList.on('endLoader', loaderContainer => loaderContainer.style.background = 'none');
    
    // 5. Show onboarding example
    // showOnboardingStories(appearanceManager: AppearanceManager, customTags?: string)
    // customTags - for override tags from storyManager
    storyManager.showOnboardingStories(appearanceManager).then(result => {
       console.log({showOnboardingStoriesResult: result});
       // result: boolean - were onboarding or not
    });
    // or window.IAS.StoryManager.getInstance()
    

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

## Onboarding only example

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
    js.src = "https://sdk.inappstory.com/v2.0.0/dist/js/storyManager.js";
    js.async = true;
    fjs.parentNode.insertBefore(js, fjs);
    st._e = [];
    st.ready = function (f) {
      st._e.push(f);
    };
    return st;
  }(document, "script", "ias-wjs"));

  // 3. This function creates an StoryManager instance (and StoriesList widget)
  //    after the API code downloads.
  window.IASReady.ready(function () {

    const storyManagerConfig = {
      apiKey: "{project-integration-key}",
      userId: "kdijhud4454d", // usually - hash from real user identifier
      tags: [], // Array<string>
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

    // 4. Show onboarding example
    // showOnboardingStories(appearanceManager: AppearanceManager, customTags?: Array<string>)
    // customTags - for override tags from storyManager
    window.IAS.StoryManager.getInstance().showOnboardingStories(appearanceManager).then(result => {
       console.log({showOnboardingStoriesResult: result});
       // result: boolean - were onboarding or not
    });

  });
  
</script>
</body>
</html>
```

---

## StoryManager public methods
```ts
type StoryManagerConfig = {
  apiKey: string;
  userId?: Optional<string|number>;
  tags?: Optional<Array<string>>;
  placeholders?: Optional<Dict<string>>;
  lang?: Optional<'ru' | 'en'>;
};

type StoryManagerCallbackPayload<T> = {src: 'storiesList' | 'storyReader', data: T};

enum StoriesEvents {
  CLICK_ON_STORY = 'clickOnStoryLink',
};

interface EventPayloadDataNameMap {
  "clickOnStoryLink": {id: number, index: number, url: string};
};

type StoryManagerCallbacks = {
  storyLinkClickHandler: (payload: StoryManagerCallbackPayload<{id: number, index: number, url: string}>) => void;
};

interface StoryManager {
  (config: StoryManagerConfig, callbacks?: StoryManagerCallbacks): StoryManager;
  getInstance(): StoryManager; // static
  setTags(tags: Array<string>): void;
  setUserId(userId: string | number): void;
  setLang(lang: 'ru' | 'en'): void;
  setPlaceholders(placeholders: Dict<string>): void;
  showStory(id: number | string, appearanceManager: AppearanceManager): Promise<boolean>;
  closeStoryReader(): void;
  showOnboardingStories(appearanceManager: AppearanceManager, customTags?: Array<string>): Promise<boolean>;
  
  // callbaks
  set storyLinkClickHandler(payload: StoryManagerCallbackPayload<{id: number, index: number, url: string}>);
  
  // events
  on<K extends keyof EventPayloadDataNameMap>(event: K, listener: (payload: StoryManagerCallbackPayload<EventPayloadDataNameMap[K]>) => void): StoryManager;
  once<K extends keyof EventPayloadDataNameMap>(event: K, listener: (payload: StoryManagerCallbackPayload<EventPayloadDataNameMap[K]>) => void): StoryManager;

}

interface StoriesList {
  (mountSelector: string, appearanceManager: AppearanceManager): StoriesList;
  reload(options: {needLoader: boolean} = {needLoader: true}): Promise<boolean>;
}
```

### StoryReader btnClickHandler example
```js
storyManager.on("clickOnStoryLink", payload => {
   const url = payload.data.url;
   if (url.indexOf('custom-schema://') === 0) {
       // run custom action
   } else {
     window.open(url, '_self');
   }
});

```


### StoriesList reload example
```js
const storiesList = new storyManager.StoriesList("#stories_widget", appearanceManager);

storyManager.setTags(['msk']);

storiesList.reload();
// or without loader animation
// storiesList.reload({needLoader: false});

```

### Show single story example
```js

// StoryManager singleton instance
const storyManager = new window.IAS.StoryManager(storyManagerConfig);

// or get previously created (from page layout for example)
const storyManager = window.IAS.StoryManager.getInstance();

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

storyManager.showStory(125, appearanceManager).then(result => {
  console.log({showStoryResult: result});
});

```


--- 

## storyManagerConfig

| Variable | Type | Description |
|----------|------|-------------|
| apiKey       | string                           | Your project integration key |
| userId       | string &#124; number &#124; null | User id |
| tags         | Array<string> | Array of tags |
| placeholders | object  | Dict for replace placeholders inside story content or title. Example: {user: "Guest"} |
| lang         | 'ru' &#124; 'en' | User locale |

## AppearanceManager - StoriesListOptions

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

### Slider favorite card additional options

| Variable | Type | Description |
|----------|------|-------------|
| title         | object | See below |
| title.content | string | Card title |
| title.color   | string | CSS valid color value. Default `#000000` |
| title.padding | number &#124; string | Number, `px` eq for all sides. <br/>String - valid css, for customizing each side. Default `15` |
| title.font    | string | CSS valid font [value](https://developer.mozilla.org/en-US/docs/Web/CSS/font). Override font. <br/>Default `normal 1rem InternalPrimaryFont` where InternalPrimaryFont - primary font, loaded in [project settings](https://console.inappstory.com). | 

### Slider navigation options

By default, controls are round buttons with arrow icons at the edges of the slider

| Variable | Type | Description |
|----------|------|-------------|
| showControls            | boolean | Enable slider controls. Default `false` |
| controlsSize            | number |  Button size, `px`. Default `48` |
| controlsBackgroundColor | string | CSS valid color value. Default `#ffffff` |
| controlsColor           | string | CSS valid color value. Default `#000000` |

## AppearanceManager - StoryReaderOptions

| Variable | Type | Description |
|----------|------|-------------|
| closeButtonPosition        | string | Close button position, one of `left`, `right` |
| scrollStyle                | string | Stories viewPager scroll style, one of `flat`, `cover`, `cube` |
| loader.default.color       | string | Default loader primary color. Valid css color |
| loader.default.accentColor | string | Default loader accent color. Valid css color |

---
