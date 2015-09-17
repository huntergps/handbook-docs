# Polyfills

FuseJS executes in a (minimum) EcmaScript 5.1 environment on all supported platforms. 
There is no web browser involved, FuseJS only provides a subset of the browser's standard libraries.

## EcmaScript features

TODO for the below: write some basic inline docs & examples, then point to the MDN docs for more info

> * : May have limited functionality compared to browser implementations

## $(fetch)

This is the main way to do HTTP requests (note Fetch and FetchJson will be renamed and de-emphasized)

TODO: * [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

> ### $(XMLHttpRequest)

https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest for more info


> ### $(Promise)

TODO: * [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)


## Polyfills

FuseJS provides polyfills for some features (typically browser features) that are mainly provided to make
$(third party libraries) work. These implementations are not neccessarily complete, but compl

> ### $(setTimeout)

TODO: * [`setTimeout`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setTimeout)


> ### $(File)* 

TODO: * [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File) *

