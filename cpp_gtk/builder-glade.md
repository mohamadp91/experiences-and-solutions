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

#### using derived widget
When you have a widget in the glade file and you want to use it in your code as class like GtkWindow, GtkApplicationWindow
and etc. 

YourWindowClass.cpp:

```
YourWindowClass::YourWindow(BaseObjectType *obj, Glib::RefPtr<Gtk::Builder> const &builder)
        : Gtk::ApplicationWindow(obj),
          builder{builder} {
    
}
```

main.cpp: 
```
int main(int argc, char *argv[]){

    auto app = Gtk::Application::create(argc, argv, "[id]");
    auto builder = Gtk::Builder::create_from_file("ui/window.glade");
    
    YourWindowClass* window = nullptr;

    builder->get_widget_derived("window", window);

    return app->run(*window);

}
```

In this case, _BaseObjectType obj*_ and _Glib::RefPtr <Gtk::Builder > const &builder_ parameters are automatically accepted
from the class in which they were created;

_**Debugging:** when your interclass signals don't work properly , 
most likely the problem is creating the instance._ when using glade and derived widget you should create instance like this:

```
YourClass *className = nullptr;
builder.get_widget_derived("class_id",className);
```