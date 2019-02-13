---
title: "Breadth-first Search: Searching trees, but sideways"
date: 2019-02-13T19:19:18Z
draft: false
---

This week was our internal "Greenlight" week at Boomi, where we get the chance to split into teams, think of an innovative project, and bash something out within 5 days.

My team (named *Steve#*, after our fearless leader's code style) decided to try and design and implement a tool for visual diffing, to make our users' lives easier when comparing differences between versions of items in our platform.

After some initial experimentation with a JSON diffing library, we found out that our attempts at rendering a nice tree by recursing from top-to-bottom, then left-to-right wasn't giving us the rendered output we desired.

My colleague Jacek threw in an idea to use something I'd never heard of before (which happens a lot, thanks to having no real CS education at university) - breadth-first searching (BFS). 

BFS is a different way of traversing a tree structure, by going left-to-right, then top-to-bottom. It's useful for... TODO. 

{{< figure src="/img/breadth-first-search.svg" caption="Alexander Drichel [CC BY 3.0 (https://creativecommons.org/licenses/by/3.0)], from Wikimedia Commons" link="https://commons.wikimedia.org/wiki/File:Breadth-first-tree.svg" position="center" title="Breadth-first search" width="800" >}}

Here's a small snippet of an implementation in Javascript (shamelessly modified from Jacek's code):

```js
function traverseByBreadth(path, item, callback) {
    // Create a queue containing the first level of items
    const queue = [{ path, item }];

    while (queue.length > 0) {

        let { path, item } = queue.shift();

        // Pass the current item to the callback function, which will probably build a structure somewhere outside of this method
        callback(path, item);

        // If this item has no children, we don't want to traverse this leaf any further
        if (!item.children) {
            continue;
        }

        // Otherwise, we want to queue this item's children for further traversal
        item.children.forEach(child => {
            queue.push({ path: child.path, item: child.item });
        });
    }
}
```

It's a pretty simple concept once you realise what's happening, and I'm sure like me, you'll think of somewhere you used something similar, but this would fit better!