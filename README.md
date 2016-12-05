# jquery.dropzone
A simple jquery plugin for creating styleable dropzones for files

* Call `$('#your-element').dropzone()` to initiate (On any element, event the `body`)
* Adds `drop-available` class to the dropzone element when a file is being dragged to the window. This is useful if you want to style your element to show that it is possible to drop the file on it.
* Adds `drop-over` class to the dropzone when the file is being dragged right over it.

Remember, you need to `preventDefault()` on the 'drop' event, or the browser will use the default action on the dropped file, which is to download it or redirect to the file.

---

It's possible to instruct it to add an extra element inside the dropzone when a file is being dragged to the window, 
which will also receive the `drop-over` class.  
This is useful for creating a dropzone on `body`, and having a styled dropzone element float above everything else.

This is done by passing an `append` option:

```javascript
$('body').dropzone({ append: '<div class="dropzone-inner">' })
```

Now just listen to 'drop' event and take the file:
```javascript
$('body')
    .dropzone({
        // This is optional, to add an internal element that you can style
        append: '<div class="full-drop-zone"><div class="description-wrapper"><div class="description">Release the file here...</div></div></div>',
        
        // This is optional, to selectively allow dropping
        allowDrop: function (event) {
            return dataTransfer.items.length > 0 && dataTransfer.items[0].kind === 'file';
        }
    })
    .on('drop', function (event) {
        event.preventDefault();

        // Do something with the files
        console.log(event.originalEvent.dataTransfer.files);
    });
```

```scss
.full-drop-zone {
  display: block;
  position: absolute;
  left: 20px;
  top: 20px;
  right: 20px;
  bottom: 20px;
  border: 2px dashed rgba(0,0,0,.3);
  border-radius: 20px;
  text-align: center;
  z-index: 20;
  background: rgba(255,255,255,0.7);
  color: rgba(0,0,0,.3);
  font-size: 20px;

  &.drop-over {
    border-color: rgba(0,0,0,0.5);
    color: rgba(0,0,0,0.5);
  }

  .description-wrapper {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .description {
    position: absolute;
    left: 50%;
    top: 50%;
    max-width: 100%;
    max-height: 100%;
    text-align: center;
    -ms-transform: translateX(-50%) translateY(-50%);
    transform: translateX(-50%) translateY(-50%);
  }
}
```


---

### Cleanup

```
$('#your-element').dropzone('destroy');
```

The plugin will be automatically cleaned up when the `remove` event is emitted (Either when jQuery.UI is present, or [jquery.removeevent](https://github.com/danielgindi/jquery.removeevent) is present).
