
# เรียกใช้งาน Location 


```dart
import 'package:location/location.dart';


onPressed: () async {

    var location = Location();
    var currentLocation = await location.getLocation();

    var locationText = '${currentLocation['latitude']} ${currentLocation['longitude']}';

    print(locationText);
    setState(() {
        _currentLocationText = locationText;
    });
},
```