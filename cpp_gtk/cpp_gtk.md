## cpp and gtkmm
___

### Instructions
___

### 1)  Implement a signal handler 

> textBuffer.signal_mark_set().connect(sigc::mem_fun(*this,&[className]::handler))`

**_textBuffer_** : you can implement text buffer with `auto preTextBuffer = Gtk::TextBuffer::create();`


1) in **_Clion_** , hold **_ctrl_** key and move mouse on signal then click on it.
Now you can see reference for implement it.

> Glib::SignalProxy< void,const TextBuffer::iterator&,const Glib::RefPtr<TextBuffer::Mark>& > signal_mark_set();`

**_void_** : type of your handler must be void

 your handler must receive two argument exactly like this `const Gtk::TextBuffer::iterator& iter ,const Glib::RefPtr<Gtk::TextBuffer::Mark>& mark`


2) you can see this reference in [**_gnome developer_**](https://developer.gnome.org/gtkmm-tutorial/).

#### Additions for this example
You can get iter of text selected with 
` textBuffer->get_selection_bounds(Gtk::TextBuffer::iterator start, Gtk::TextBuffer::iterator end);`
___

#### Override
Another way to implement a signal is override it.
* your current class must be extended from class that include the signal.
* you don't need to implement **_connect_** and etc just you should implement signal in header file and source file.

For example:

```
...

class draw : public Gtk::DrawingArea {

...

protected:

     bool on_draw(const Cairo::RefPtr<Cairo::Context> &cr);

```

source code :
```
     bool className:: on_draw(const Cairo::RefPtr<Cairo::Context> &cr){
     ...
     };

```
___
#### short functions 
```
   button.signal_button_release_event().connect([&](GdkEventButton*){
        drawer.draw_rectangle();
        return true;
    });
```

### Gtk::Grid

> Gtk::Grid::attach(widget,left,top,width,height);

Left specifies the column in which the widget should be placed.
Top specifies the row in which the widget should be placed.

For example :
> Gtk::Grid::attach(widget,1,1,1,1);

### add_event()
The event mask of the widget determines what kind of events will a particular widget receive. Some event are 
preconfigured, other events have to be added to the event mask.
Like :
> drawingArea.add_event(Gdk::BUTTON_PRESS_MASK );

Then call a signal handler to handle this signal:

>    signal_button_press_event().connect([&](GdkEventButton* e){return true; });

#### select a region with mouse to draw the shapes
* When you set **_GDK_POINTER_MOTION_MASK_** => you receive every mouse move. but you don't need this and you shouldn't use this.
~~add_event(GDK_POINTER_MOTION_MASK)~~. but you need the signal handler of this signal its called _**signal_motion_notify_event**_.
  In this case you should do that like this:
  
```
  widget.add_events(Gdk::BUTTON1_MOTION_MASK | Gdk::BUTTON_PRESS_MASK);
    widget.signal_motion_notify_event().connect([&](GdkEventMotion* e){
        return true;
    });
```
#### Gdk::BUTTON1_MOTION_MASK
Receive pointer motion events while 1 button is pressed

#### Gdk::BUTTON_PRESS_MASK
Receive button press events

[more details about another signals](https://developer.gimp.org/api/2.0/gdk/gdk-Events.html)

#### loading css file
```
 auto css = Gtk::CssProvider::create();
    css->load_from_path("style/file.css");
    window1->get_style_context()->add_provider_for_screen(Gdk::Screen::get_default(),
                                                          css,
                                                          GTK_STYLE_PROVIDER_PRIORITY_APPLICATION);
```
