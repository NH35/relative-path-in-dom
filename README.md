# Function to get relative path between 2 nodes elements in the DOM
JS tool function for userscript developpement to easily get the relative path (aka .parentNode.childNodes.item(i) ) between 2 nodes.

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

## Demo Exemple 
![Alt demo](./demo.gif)

