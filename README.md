# Function to get relative path between 2 nodes elements in the DOM
Provide a String made of ".parentNode" and ".childNodes.item(i)" of the shortest path between the two nodes

## What the purpose of this ?
It's usefull to dev quickly when you have same div structure located at different places in the dom. 
Like for example : the reddit comment section. You get every div comment with a querySelectorAll and want to downvote all of them. Problem : the comment section is recursive so every comments are not clean ul. But you know the downvote button keep the same relative position for each comment. So you use this function to know the relative path and then make a for loop to downvote all of them.
Yes, you can still inspect the dom manually to know the relative path but it's just faster.

## Code
```js
// Function designed to be a tool during the development of a JS userscript to get the relative path from an DOM element to another element.
// return a String made of ".parentNode" and ".childNodes.item(i)" of the shortest path between the two nodes.
// NH35 - 17/07/2022
function getRelativePath(startElement, stopElement, max_dept = null) {

    function explore(current, from, stopElement, path, max_dept, dept) { // recursive exploration function

        if (max_dept && dept >= max_dept) return null //exit if user has define a maximum dept of recursion

        // return path if found
        if (current === stopElement) {
            return path
        }

        // explore childrend before going above so you always find the shortest path
        for (var i = 0; i < current.childNodes.length; i++) {
            if (from !== current.childNodes[i]) { // we exclude the path we came from because it's alreday explored
                let res = explore(current.childNodes[i], current, stopElement, path + `.childNodes.item(${i})`, max_dept, dept + 1)
                if (res) return res
            }
        }

        // explore parent
        if (from !== current.parentNode && current.parentNode !== null) {
            let res = explore(current.parentNode, current, stopElement, path + `.parentNode`, max_dept, dept + 1)
            if (res) return res
        }

        return null // nothing found at the bottom of the branch
    }

    return explore(startElement, startElement, stopElement, "", max_dept, 0)
}
```

## Demo Example 
![Alt demo](./demo.gif)

