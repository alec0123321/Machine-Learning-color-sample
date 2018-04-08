# Brain.js 類神經網路 JS應用
> 這個應用是因為看到youtube上有人分享了很酷的視頻，決定跟著做做看，非原創
> [reference1](https://www.youtube.com/watch?v=9Hz3P1VgLz4&t=3s)
> [brain.js](https://github.com/BrainJS/brain.js)

## 基本式
> 透過建立一個新NeuralNetwork後給予訓練的data，在這邊的例子是利用顏色rgb color來當作訓練的data
> 如果顏色是深色，我們的output就會寫dark:1，如果我們的顏色是亮色的，output就會寫light: 1
> 透過訓練模型讓input的資料透過這些data去得到dark的機率及light的機率，透過brain.likely得到機率最高的那個值
>> e.g. dark: 0.988888, light: 0.211111，最後會得到dark的值

> 所以這個範例是透過得到背景是light或是dark來決定Text color

```js
const network = new brain.NeuralNetwork()

network.train([
    { input: { r: 0.62, g: 0.72, b: 0.88 }, output: { light: 1 } },
    { input: { r: 0.1, g: 0.84, b: 0.72 }, output: { light: 1 } },
    { input: { r: 0.33, g: 0.24, b: 0.29 }, output: { dark: 1 } },
    { input: { r: 0.74, g: 0.78, b: 0.86 }, output: { light: 1 } },
    { input: { r: 0.31, g: 0.35, b: 0.41 }, output: { dark: 1 } },
    { input: {r: 1, g: 0.99, b: 0}, output: { light: 1 } },
    { input: {r: 1, g: 0.42, b: 0.52}, output: { dark: 1 } },
  ])

const result = brain.likely(rgb, network)

```

## 完整範例
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Color Brain Test</title>
  <link rel="stylesheet" href="/css/style.css">
  <script src="https://unpkg.com/brain.js@1.1.2/browser.min.js"></script>
  <!-- <script src="./js/color.js" charset="utf-8"></script> -->
</head>
<body>
  <div class="box">
    <input type="color" value="#ff0000"/>
    <div id="example">Text color will change when background color turn bright</div>
  </div>
    <script src="./js/color.js" charset="utf-8"></script>
</body>
</html>
```
``` js
const input = document.querySelector("input")
const example = document.querySelector("#example")

input.addEventListener("change", (e) => {
  const rgb = getRgb(e.target.value);
  const network = new brain.NeuralNetwork()
  console.log(rgb);
  network.train([
    { input: { r: 0.62, g: 0.72, b: 0.88 }, output: { light: 1 } },
    { input: { r: 0.1, g: 0.84, b: 0.72 }, output: { light: 1 } },
    { input: { r: 0.33, g: 0.24, b: 0.29 }, output: { dark: 1 } },
    { input: { r: 0.74, g: 0.78, b: 0.86 }, output: { light: 1 } },
    { input: { r: 0.31, g: 0.35, b: 0.41 }, output: { dark: 1 } },
    { input: {r: 1, g: 0.99, b: 0}, output: { light: 1 } },
    { input: {r: 1, g: 0.42, b: 0.52}, output: { dark: 1 } },
  ])

  const result = brain.likely(rgb, network)
  console.log(result);
  example.style.background = e.target.value
  example.style.color = result === "dark" ? "white" : "black"
})

// color get function learn from LearnCode.academy
//https://www.youtube.com/watch?v=9Hz3P1VgLz4&t=3s
function getRgb(hex) {
  var shorthandRegex = /^#?([a-f\d])([a-f\d])([a-f\d])$/i;
  hex = hex.replace(shorthandRegex, function(m, r, g, b) {
      return r + r + g + g + b + b;
      // console.log(r + r + g + g + b + b)
  });

  var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
  return result ? {
      r:
    Math.round(parseInt(result[1], 16)/ 2.55) / 100,
      g: Math.round(parseInt(result[2], 16) / 2.55) / 100,
      b: Math.round(parseInt(result[3], 16) / 2.55) / 100,
  } : null;
}

```
