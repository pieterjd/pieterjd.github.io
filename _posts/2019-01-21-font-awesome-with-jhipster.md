---
layout: single
title:  "Font Awesome with JHipster"
date:   2019-01-22 19:24:21 +0100
excerpt: "JHipster includes Font Awesome, a great icon library. How to do more with the icons you need."
categories: [tech]
tags: [JHipster, Font Awesome]
published: true
---
[Font Awesome](https://fontawesome.com/) offers vector icons and social logos. As it is one of the most popular icon sets, it is included with JHipster.

Out of the box, only some icons are available for your angular front-end. And it's a bit tricky to find out how these icons are loaded as all Angular components use the ``fa-icon`` directive, available in JHipster.

In this particular use-case, I need to add the shopping basket icon. After some searching around, I found out all ~~Font~~ [Fort Awesome](https://fortawesome.com/) icons are loaded in ``src/main/webapp/app/vendor.ts``. So you just load in the icon of your choice:

```javascript
import {
    faUser,
    faSort,
    //snip
    faShoppingBasket
} from '@fortawesome/free-solid-svg-icons';
//snip
library.add(faShoppingBasket);
```

Finally in your component's template, you can include the icon with ``<fa-icon icon="shopping-basket"></fa-icon>``.
