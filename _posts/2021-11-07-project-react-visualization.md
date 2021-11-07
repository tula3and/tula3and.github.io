---
title: "Visualize charts with React"
categories:
  - Project
tags:
  - Project
  - Web
  - React
  - JSX
last_modified_at: 2021-11-07
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

As a part of [cloud-ready](https://github.com/qiskit-advocate/qamp-fall-21/issues/16),
I need to visualize the result to graphs in the frontend.

## JSON data into graphs

The result file will be a json file so I need to import a json data for visualization.
It was simple with the function `map`. This returns each object in an array, so I used it to display a table.

I chose [nivo](https://github.com/plouc/nivo) for the visualization tool.
This library supports so many types of graphs and it is also easy to check codes interactively at [nivo.rocks](https://nivo.rocks/).
For installation, just execute one command line below. (Note that installation with `yarn` is in the offical repository.)

```
npm install --save @nivo/core @nivo/bar
```

I use `@nivo/bar` in this case, if you want another chart, please change that part.

## Conditional rendering

There is a unique rendering method in React called conditional rendering.
It uses a boolean type value to select what to show.

- Inline if with logical `&&` operator
  ```js
  {<boolean> && <component>}
  ```
- Inline if-else with conditional operator
  ```js
  {<boolean> ? 'on' : 'off'}
  ```

I used these things to switch between two charts.

```js
<Button onClick={() => {setBar(!myBar), setTable(!myTable)}}>
  {myBar ? 'Show Table' : 'Show Bar'}
</Button>

{myTable && (
  <MyTable />
)}

{myBar && (
  <>
  <Button onClick={() => setdataA(!dataA)}>
    {dataA ? 'Show Banana' : 'Show Apple'}
  </Button>
  {dataA && (
  <MyBar sampleJson={appleList}/>
  )}
  {!dataA && (
  <MyBar sampleJson={bananaList}/>
  )}
  </>
)}
```

There are totally two buttons. One is for switching between table and bar
and the other is visiable only for a bar to change data.
Do not forget to add `<>` and `</>` at both sides to prevent the error: `JSX expressions must have one parent element`.

## Design with Carbon

Carbon is IBM’s open source design system for products and digital experiences.
You can check available React components at this website: [react.carbondesignsystem.com](https://react.carbondesignsystem.com/?path=/story/getting-started--welcome).
I organized table and button design with this. It is convenient that I do not have to write css.

The final result looks like this: ![gif](https://user-images.githubusercontent.com/62553200/140605176-45ac350e-1263-4e99-a769-90d003ae577c.gif)

## References

- [Top 11 React Chart Libraries](https://www.tabnine.com/blog/top-11-react-chart-libraries/)
- [Introducing JSX](https://reactjs.org/docs/introducing-jsx.html)
- [Conditional Rendering](https://reactjs.org/docs/conditional-rendering.html)
- [Carbon Design System](https://www.carbondesignsystem.com/)

---

💬 _Any comments and suggestions will be appreciated._
