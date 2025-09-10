```javascript
const toggleImportanceOf = id => {
  const url = `http://localhost:3001/notes/${id}`
  const note = notes.find(n => n.id === id)
  const changedNote = { ...note, important: !note.important }

  axios.put(url, changedNote).then(response => {
    setNotes(notes.map(note => note.id === id ? response.data : note))
  })
}
```
___
`currentColor` keyword
```html
<div class="container">
  The color of this text is blue.
  <div class="child"></div>
  This block is surrounded by a blue border.
</div>

<style>
    .container {
        color: blue;
        border: 1px dashed currentColor;
    }
    .child {
        background: currentColor;
        height: 9px;
    }
</style>
```
___