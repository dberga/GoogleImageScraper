# **Note: Check out latest working Google Image Scraper in [https://github.com/feceugur/GoogleImageScraper.git](https://github.com/feceugur/GoogleImageScraper.git)**

# Google Image Scraper
<div align="center"><img src="https://raw.githubusercontent.com/commonkestrel/GoogleImageScraper/master/misc/logo.png" width=640px></div>

This is a library for retrieving and downloading images from Google Images.  
It uses an input query and arguments to search and retrive image objects.
These images may be protected under copyright, and you should not do anything punishable with them, like using them for commercial use.
This library is inspired by ```google-images-download``` by **hardikvasa**, but adds a few quality of life improvments, such as being able to retrieve urls as well. This library would not be possible, however, without their work, and the people who are working to continue it.

# Arguments
There is one required argument and two arguments in both of the main functions:
| Argument | Types | Description |
| --- | --- | --- |
| **query:** | *str, list* | Either a string or list containing the keywords to search for. If the query is a string, it will be separated into different keywords by spaces.  
| **limit** | *int* | The amount of images to search for. Cannot be greater then 100. *\*Defaults to 1\**  
| **arguments:** | *dict* | This is a dictionary containing many optional values, all of which will be listed here. They are split into two categories: *Search arguments* and *Download arguments* 

## Download arguments
| Argument | Types | Description |
| --- | --- | --- |
| **download_format** | *str* | Specifies a file extension to download all images as. Must be a valid image file extension recognized by *PIL*. ***\*Note:*** *This takes considerably longer with large amounts of images\**
| **directory** | *str* | This specifies the directory name to download the images to. This will automatically be created in the directory the function is called in, unless the directory already exists or **path** is specified. 
| **path** | *str* | This specifies the path to create the download directory in.  
| **timeout** | *int float* | This specifies the maximum time the program will wait to retrieve a single image in seconds.
| **verbose** | *bool* | Set to ```True``` in order to print updates on progress to the console.


## Search Arguments

| Argument | Accepted Values | Description |
| --- | --- | --- |
| **color** | *'red', 'orange', 'yellow', 'green', 'teal', 'blue', 'purple', 'pink', 'white', 'gray', 'black', 'brown'* | Filter images by the dominant color. |
| **color_type** | *'full', 'grayscale', 'transparent'* | Filter images by the color type, full color, grayscale, or transparent. |
| **license** | *'creative_commons', 'other_licenses'* | Filter images by the usage license. |
| **type** | *'face', 'photo', 'clipart', 'lineart', 'gif'* | Filters by the type of images to search for. \**Not to be confused with search_format*\* |
| **time** | *'past_day', 'past_week', 'past_month', 'past_year'* | Only finds images posted in the time specified. |
**aspect_ratio** | *'tall', 'square', 'wide', 'panoramic'* | Specifies the aspect ratio of the images. |
**search_format** | 'jpg', 'gif', 'png', 'bmp', 'svg', 'webp', 'ico', 'raw' | Filters out images that are not a specified format. If you would like to download images as a specific format, use the 'download_format' argument instead. |

# Usage
There are four available functions, **download**, **urls**,  **image_objects** and **download_image**, which works differently than the others:

## download:

```python
import GoogleImageScraper

images = GoogleImageScraper(query, limit, arguments)
```
This will download images based on the arguments.
The returned values will follow this format:
```python
{'images': [images], 'errors': Number of Errors}
```
Each of the images in the list of images will follow a particular format as well:
```python
{'path': Image Path, 'url': Image Url}
```

---

## urls:
```python
import GoogleImageScraper

urls = GoogleImageScraper.urls(query, limit, arguments)
```

This function simply returns a list of image urls from the search terms.

___

## image_objects:
This function is a little more niche, but it may be useful to some people. Instead of returning a list of image urls like with the **urls** function, it returns a list of image objects containing useful data, structured like so:

```python
{'url': Image url, 'thumbnail': Url of image thumbnail, 'source_url': The webpage the image was found on, 'source': The base url of the source}
```
The usage is similar to the previous functions:
```python
import GoogleImageScraper

image_objects = GoogleImageScraper.image_objects(query, limit, arguments)
```

---

## download_image:
Use this function to download an image via url. This function is different from the rest in that it takes different input arguments, provided below:

| Argument | Types | Description |
| --- | --- | --- |
| url | *str* | The url to download the image from. *\*required\** |
| name | *str* | The name of the file. **Do not include file extension.** *\*required\**
| path | *str* | The path to download the image to. |
| download_format | *str* | The format to download the image in. *Takes a while longer* |
| overwrite | *bool* | Whether to overwrite files with the same name. *Defaults to ```True```.* Raises ```FileExistsError``` if ```False``` and the file exists. |

# Errors
There is a chance that you may not reach the number of images specified in the *limit* argument. This occurs when there is an error downloading an image, whether it is not in an image format, or the request times out, it can happen. When downloading a large amount of images, this may cause your limit to not be reached. The *'errors'* item in the returned dictionary from downloads is your way of keeping track of that. For example, if your *limit* was 100, and 3 images threw errors, you would get 97 images back, and the *'errors'* item would be 3. Now, if your *limit* was 20, however, and 3 images threw errors, you would still get 20 items back, and the *'errors'* item would be 0. This is because a max of 100 urls can be found in one query, so higher limits increases the chance that errors will cut into your *limit*.

## Included Errors
| Error | Description|
| --- | --- |
| ```LimitError``` | Raised when the limit argument is above 100 or not the proper type. |
| ```ArgumentError``` | Raised when an invalid value is given for an argument | 
| ```QueryError``` | Raised if there is no query or the query is not the proper type | 
| ```UnpackError``` | Raised if no images are found on the page. | 
| ```DownloadError``` | Exclusive to the *download_image* function. Raised if the image failed to download. |

Include these like so:
```python
from GoogleImageScraper.errors import <error>
```

# Examples:
A few real examples are listed here:
## urls:
```python
import GoogleImageScraper
urls = GoogleImageScraper.urls(query='cats', limit=10, arguments={'color': 'black'})
```
Result:
```python
['https://www.rd.com/wp-content/uploads/2021/01/GettyImages-1175550351.jpg', 
'https://www.history.com/.image/ar_4:3%2Cc_fill%2Ccs_srgb%2Cfl_progressive%2Cq_auto:good%2Cw_1200/MTg0NTEzNzgyNTMyNDE2OTk5/black-cat-gettyimages-901574784.jpg', 
'https://www.thesprucepets.com/thmb/kF3_dQW_JT1ClMQDlISxq3BgeT4=/6843x5132/smart/filters:no_upscale()/facts-about-black-cats-554102-hero-7281a22d75584d448290c359780c2ead.jpg', 
'https://i.guim.co.uk/img/media/c5e73ed8e8325d7e79babf8f1ebbd9adc0d95409/2_5_1754_1053/master/1754.jpg?width=465&quality=45&auto=format&fit=max&dpr=2&s=065f279099ded1062688e357b155dc29', 
'https://cdn.cnn.com/cnnnext/dam/assets/141030105303-kiki-irpt.jpg', 
'https://imagesvc.meredithcorp.io/v3/mm/image?url=https%3A%2F%2Fstatic.onecms.io%2Fwp-content%2Fuploads%2Fsites%2F34%2F2021%2F09%2F27%2Fblack-cat-kitchen-rug-getty-0921-2000.jpg', 
'https://www.gannett-cdn.com/presto/2021/10/28/USAT/1bf79c6a-5d88-4e64-b398-c40418a79829-XXX_iStock_000017680551Large.jpg',
'https://cdn.sanity.io/images/0vv8moc6/dvm360/f28cc9b680aed62edd018ce47a5cbb96c4f78f3b-4860x3024.jpg', 
'https://vbspca.com/wp-content/uploads/2019/10/Image-e1570199876255.jpeg', 
'https://ichef.bbci.co.uk/news/976/cpsprodpb/AECE/production/_99805744_gettyimages-625757214.jpg']
```

---
## download:
```python
import GoogleImageScraper
images = GoogleImageScraper.download(query='dogs', limit=1, arguments={'color':'brown', 'download_format': 'png'})
```
Result:
```python
{'images': [{'path': '<path>\\images\\dogs-0.png', 'url': 'https://post.medicalnewstoday.com/wp-content/uploads/sites/3/2020/02/322868_1100-800x825.jpg'}], 'errors': 0}
```

---

## image_objects:

```python
import GoogleImageScraper
objects = GoogleImageScraper.image_objects(query='birds', limit=1, arguments={'color':'yellow'})
```
Results:
```python
[{'thumbnail': 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQwDI5y3_n2rwFQLZKrBXs5VL_J38zlZVvdZAooD8F8d7lY8ZA9iLEb1-AoBBWpGftpdoc&usqp=CAU', 'url': 'https://www.sfvaudubon.org/wp-content/uploads/2020/03/YEWAcrop.jpg', 'source_url': 'https://www.sfvaudubon.org/sfv-backyard-bird-identification/', 'source': 'sfvaudubon.org'}, {'thumbnail': 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR1k5IhGCAPgU468tyPrgkuY9WC3T83zRxzFrTOOUs0OL_kanPG8VPKXV3euijAlzW9AsE&usqp=CAU', 'url': 'https://ca.audubon.org/sites/default/files/styles/article_teaser/public/yellowwarbler_peter_latourrette.jpg?itok=PFRtxcGN', 'source_url': 'https://ca.audubon.org/birds-0', 'source': 'ca.audubon.org'}]
```
