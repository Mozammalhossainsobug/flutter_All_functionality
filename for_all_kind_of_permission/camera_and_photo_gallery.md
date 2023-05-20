# To get Camera Access

>add image_picker package

### Android (AndroidManifest.xml)
1. Open the    *android/app/src/main/AndroidManifest.xml*     file in your Flutter project.

2. Add the following permissions inside the <manifest> tag:
```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```
3. Additionally, if you want to support devices with Android 10 (API level 29) or higher, you need to request the legacy external storage permission by adding the following attribute to the <application> tag:
  ```
  android:requestLegacyExternalStorage="true"
  ```

  ### iOS (Info.plist)
1. Open the     *ios/Runner/Info.plist*   file in your Flutter project.
2. Add the following keys under the <dict> tag:
```
<key>NSCameraUsageDescription</key>
<string>Access to the camera is required to capture photos.</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>Access to the photo library is required to choose photos.</string>
```
  
 ### Demo Code
  ```
  import 'dart:io';
import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';

class HomePage extends StatefulWidget {
  const HomePage({Key? key}) : super(key: key);

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  File? _imageFile;

  Future<void> _getImage(ImageSource source) async {
    final picker = ImagePicker();
    final pickedFile = await picker.getImage(source: source);

    if (pickedFile != null) {
      setState(() {
        _imageFile = File(pickedFile.path);
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Home Page')),
      body: Column(
        children: [
          ElevatedButton(
            onPressed: () {
              _getImage(ImageSource.camera);
            },
            child: const Text('Take Photo'),
          ),
          ElevatedButton(
            onPressed: () {
              _getImage(ImageSource.gallery);
            },
            child: const Text('Choose from Gallery'),
          ),
          if (_imageFile != null)
            CircleAvatar(
              radius: 80,
              backgroundImage: FileImage(_imageFile!),
            ),
        ],
      ),
    );
  }
}
  ```

