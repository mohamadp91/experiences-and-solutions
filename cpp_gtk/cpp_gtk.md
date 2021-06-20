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