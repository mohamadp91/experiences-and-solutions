## epubjs library
This library is used to read Epub books in interface mod.
___
First import **_epubjs_** and then:
```
import eBook from "[your epub book]"

const book = epub(eBook)
const rendition = book.renderTo("[element id]", {
			width: 800,
			height: 800,
			flow: "paginated",
		})
		rendition.display()
```
Now you can see your Epub book on browser.
To go to next and previous page:
```
rendition.next()
```
```
rendition.prev()
```