# Use Naive Element

```html
<n-config-provider :theme="theme">
  <n-button @click="theme = 'dark'">Dark</n-button>
  <n-button @click="theme = 'light'">Light</n-button>
  <n-el as="span" class="oops">
    I am a span
  </n-el>
</n-config-provider>
```
```js
export default {
  data () {
    return {
      theme: 'dark'
    }
  }
}
```
```css
.oops {
  color: black;
  transition: color .3s cubic-bezier(.4, 0, .2, 1);
}
.oops.n-light-theme {
  color: green
}
.oops.n-dark-theme {
  color: yellow
}
.n-button {
  margin: 0 12px 8px 0;
}
```