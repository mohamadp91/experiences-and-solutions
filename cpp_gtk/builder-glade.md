## Gtk::Builder and Glade


### add .glade file to project scope and handle it with cmake 
> file(COPY [ui file/ dir] DESTINATION ${CMAKE_CACHEFILE_DIR})

### Introduce the .glade file
> auto builder = Gtk::Builder::create_from_file("fileName.glade");

#### get widget from .glade file
If you don't want to add widget to another widget:

```
    auto widget = Glib::RefPtr<Gtk::your_widget>::cast_dynamic(builder->get_object("ui_id"));
```

If you want to add widget to another widget:
```
    Gtk::YourWidget *widget_name = nullptr;
    builder->get_widget<Gtk::YourWidget>("ui_id",widget_name);
    add(*widget_name);
```