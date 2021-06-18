# Opencv JS separate wasm build

Build of opencv.js with separate js and wasm files

Version of this npm package corresponds with opencv version

## How to use?
- install with npm `npm install @vitalyros/opencvjs-separate-wasm`
- make `node_modules/@vitalyros/opencvjs-separate-wasm/opecvjs.wasm` file accessible for [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) requests. This depends on where you want to store this wasm file. If you don't need to store it separately from js file, then why are you using this? Use [the common package](https://www.npmjs.com/package/@opencv.js/wasm).
- In your javascript
    - import default from javascript part of the package `import {default as initOpencvJs} from "@vitalyros/opencvjs-separate-wasm";` FYI this is a factory function see MODULARIZE=1 in [emscripten docs](https://emscripten.org/docs/getting_started/FAQ.html)
    - fetch wasm `let wasm = await fetch("http://mysite/opencvjs.wasm");`
    - turn wasm into a buffer `let buffer = await wasm.arrayBuffer();`
    - initialize opencv.js `cv = await initOpencvJs({wasmBinary: buffer});`
    - use cv as described in [opencv.js docs](https://docs.opencv.org/4.5.2/d5/d10/tutorial_js_root.html)

e.g. for wasm stored as separate file in web extension
```
import {default as initOpencvJs} from "@vitalyros/opencvjs-separate-wasm";

var cv;

(async() => {
    let url = browser.extension.getURL("./dist/ext/opencv/opencv.wasm");
    let wasm = await fetch(url);
    let buffer = await wasm.arrayBuffer();
    cv = await initOpencvJs({
        wasmBinary: buffer
    });
    // cv.imread(image)
})();
```