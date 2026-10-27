# YoYo - Friends
 
A **Vue 3** web app for collecting and sharing videos from the YoYo community. YoYo is a platform that makes yoga accessible to everyone, regardless of physical ability.
 
Project built for the start2impact exam.

 ## Run with Netlify

If you want to try the web app : https://yoyo-yoga-app.netlify.app/



## Features
 
- **"Friends" home page**: a list of the community's videos, loaded from an API with Axios.
- **Infinite scroll**: videos appear 3 at a time; when the user reaches the bottom of the list, 3 more are loaded, until none are left.
- **Video page**: an embedded YouTube player, with title, author, view count and description.
- **Comments**: below the video there is a comments area. A comment written in the form appears in the list above it. Comments are kept separately for each video.
- **Responsive**: designed mobile-first, with a card grid that also adapts to desktop screens.

## Tech stack
 
| Technology | Purpose |
| --- | --- |
| Vue 3 (`<script setup>`) | App framework |
| Axios | AJAX request to the videos API |
| Bootstrap Vue Next | UI components (cards, buttons) and layout |
| vue3-youtube | YouTube video embed |
| IntersectionObserver (browser API) | Detecting the end of the list for infinite scroll |
 
## API
 
The data comes from the `videos.json` file in the [yoyo-friends-api](https://anita-liberatore.github.io/yoyo-friends-api/) repository.
Each video has these fields: `id`, `title`, `author`, `videoId`, `thumbnail`, `duration`, `views`, `description`.
 
The API returns all the videos at once, so the "3 at a time" split is done on the client side.
 
## Project structure
 
```
src/
├── App.vue                      # global state, header, screen selection
└── components/
    ├── VideoList.vue            # video cards + infinite scroll
    ├── intersectionObserver.vue # sentinel: signals when it becomes visible
    ├── VideoPage.vue            # player and video details
    └── CommentSection.vue       # comment list and form
```
 
There are 5 components in total, each well under 400 lines.
 
### Component responsibilities
 
- **App.vue**: loads the data with Axios, holds the state (`videos`, `selectedVideo`, `comments`) and decides whether to show the list or the video page.
- **VideoList.vue**: displays the cards for the visible videos and increases their number when the sentinel asks for it.
- **intersectionObserver.vue**: watches an empty element at the bottom of the list and emits an event when it enters the screen. It knows nothing about the data.
- **VideoPage.vue**: displays the player and the information of the selected video.
- **CommentSection.vue**: displays the comments and handles the input form.

### Event flow
 
Data flows down from parent to child components through **props**, while events flow up from children to parents through **emit**.
 
| Event | Emitted by | Received by | Effect |
| --- | --- | --- | --- |
| `visible` | intersectionObserver | VideoList | shows 3 more videos |
| `select` | VideoList | App | sets `selectedVideo` and opens the video page |
| `back` | VideoPage | App | clears `selectedVideo` and returns to the list |
| `submit` | CommentSection | VideoPage | forwards the comment text |
| `comment` | VideoPage | App | saves the comment for the current video |
 
## Choices to keep the code simple
 
- No router and no store: with only two screens, a single `ref` (`selectedVideo`) is enough to decide what to show.
- All shared state lives in `App.vue`, so there is only one place to look for the data.
- Child components receive props and emit events, without directly modifying their parents' state.
- The visible list is not a second array to keep in sync: it is a `computed` that slices the original array with `slice`.
- The YouTube embed is handled by a library, instead of managing the player API by hand.
