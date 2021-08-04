## vala lang

### display an image and make that to follow the cursor with default size
```
using Gtk;

public class MyWindow : Gtk.Window {

private Image image;


	public MyWindow () {

	// window configuration
	this.set_title ("Image Viewer in Vala");
    this.border_width = 10;
    this.window_position = WindowPosition.CENTER;
    this.set_default_size (500, 500);
	this.set_events(Gdk.EventMask.POINTER_MOTION_MASK );

    // headbar configuration
	Gtk.HeaderBar header = new Gtk.HeaderBar();
	var button = new Button.with_label ("Open image");
    header.pack_start(button);
    header.set_show_close_button(true);
    this.set_titlebar(header);

    Gtk.Fixed fix = new Gtk.Fixed();

	button.clicked.connect (on_open_image);
    image = new Image();
    fix.put(image,50,50);
    this.add (fix);
         this.motion_notify_event.connect  ((event) => {
             fix.move(image, (int) event.x,(int) event.y);
            return false;
        });
 }
     // dialog
    public void on_open_image (Button self) {
    	var filter = new FileFilter ();
	    var window = new Gtk.Window();
	    var dialog = new FileChooserDialog ("Open image",
	                                    window,
	                                    FileChooserAction.OPEN,
	                                    Stock.OK,
	                                    ResponseType.ACCEPT,
	                                    Stock.CANCEL,
	                                    ResponseType.CANCEL);
	filter.add_pixbuf_formats ();
	dialog.add_filter (filter);

	switch (dialog.run ())
	{
		case ResponseType.ACCEPT:
			var filename = dialog.get_filename ();
			try{
			Gdk.Pixbuf img = new Gdk.Pixbuf.from_file_at_size(filename,300,300);
			image.set_from_pixbuf(img);
			}
			catch{
			print("file not found");
			}

			break;
		default:
			break;
	}
	dialog.destroy ();
    }
}
```