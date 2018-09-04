Piexifjs
========

[![Build Status](https://travis-ci.org/hMatoba/piexifjs.svg?branch=master)](https://travis-ci.org/hMatoba/piexifjs)

[![CircleCI](https://circleci.com/gh/hMatoba/piexifjs/tree/master.svg?style=svg)](https://circleci.com/gh/hMatoba/piexifjs/tree/master)

Notice and Warning!
-------------------

We are implementing v2.0. This version would include a few big changes. If you won't ready to use, don't update this library.
 
```
npm install piexifjs@1.0.4
```
 
Thank you for using piexifjs!

version 2.0
-----------

- add some arguments type checks 
- stop to support bower
- some data types are changed in exif?

How to Use
----------

- *var exifObj = piexif.load(jpegData)* - Get exif data as *object*. *jpegData* must be a *string* that starts with "\data:image/jpeg;base64,"(DataURL), "\\xff\\xd8", or "Exif".
- *var exifStr = piexif.dump(exifObj)* - Get exif as *string* to insert into JPEG.
- *piexif.insert(exifStr, jpegData)* - Insert exif into JPEG. If *jpegData* is DataURL, returns JPEG as DataURL. Else if *jpegData* is binary as *string*, returns JPEG as binary as *string*.
- *piexif.remove(jpegData)* - Remove exif from JPEG. If *jpegData* is DataURL, returns JPEG as DataURL. Else if *jpegData* is binary as *string*, returns JPEG as binary as *string*.

Use with File API or Canvas API.

Example
-------

    <input type="file" id="files" />
    <script src="/js/piexif.js" />
    <script>
    function handleFileSelect(evt) {
        var file = evt.target.files[0];
        
        var zeroth = {};
        var exif = {};
        var gps = {};
        zeroth[piexif.ImageIFD.Make] = "Make";
        zeroth[piexif.ImageIFD.XResolution] = [777, 1];
        zeroth[piexif.ImageIFD.YResolution] = [777, 1];
        zeroth[piexif.ImageIFD.Software] = "Piexifjs";
        exif[piexif.ExifIFD.DateTimeOriginal] = "2010:10:10 10:10:10";
        exif[piexif.ExifIFD.LensMake] = "LensMake";
        exif[piexif.ExifIFD.Sharpness] = 777;
        exif[piexif.ExifIFD.LensSpecification] = [[1, 1], [1, 1], [1, 1], [1, 1]];
        gps[piexif.GPSIFD.GPSVersionID] = [7, 7, 7, 7];
        gps[piexif.GPSIFD.GPSDateStamp] = "1999:99:99 99:99:99";
        var exifObj = {"0th":zeroth, "Exif":exif, "GPS":gps};
        var exifStr = piexif.dump(exifObj);

        var reader = new FileReader();
        reader.onload = function(e) {
            var inserted = piexif.insert(exifStr, e.target.result);

            var image = new Image();
            image.src = inserted;
            image.width = 200;
            var el = $("<div></div>").append(image);
            $("#resized").prepend(el);

        };
        reader.readAsDataURL(file);
    }
    
    document.getElementById('files').addEventListener('change', handleFileSelect, false);
    </script>

Dependency
----------

No dependency. Piexifjs just needs standard JavaScript environment.

Environment
-----------

Both client-side and server-side. Standard browsers(without IE) and Node.js.

Issues
------

Give me details. Environment, code, input, output. I can do nothing with abstract.

License
-------

This software is released under the MIT License, see LICENSE.txt.