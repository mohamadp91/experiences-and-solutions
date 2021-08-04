## [Cairo](https://www.cairographics.org/documentation/cairomm/reference/examples.html)
___

Cairo drawing must be added to a **_Gtk::DrawingArea_**.
```
Gtk::DrawingArea myArea;
Cairo::RefPtr<Cairo::Context> myContext = myArea.get_window()->create_cairo_context();
myContext.save();
myContext->set_source_rgb(1.0, 0.0, 0.0);
myContext->set_line_width(2.0);
myContext.restore();

myContext.preserve();


```

* It is good practice to put all modifications to the graphics state between save()/restore()
function calls. For example, if you have a function that takes a Cairo::Context reference as an
argument, you might implement it as follows:

* Many Cairo drawing functions have a _preserve() variant. Normally drawing functions such as
  clip(), fill(), or stroke() will clear the current path. If you use the _preserve() variant, the
  current path will be retained so that you can use the same path with the next drawing function