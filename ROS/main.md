### 从Mat 到发布图像消息
```
#include <ros/ros.h>
#include <image_transport/image_transport.h>
#include <opencv2/highgui/highgui.hpp>
#include <cv_bridge/cv_bridge.h>


image_transport::ImageTransport it(nh);
image_transport::Publisher pub = it.advertise("camera/image", 1);

sensor_msgs::ImagePtr msg = cv_bridge::CvImage(std_msgs::Header(), "bgr8", image).toImageMsg();

pub.publish(msg);
```

#### CMakeLists.txt
```
find_package(
    image_transport 
    cv_bridge)
```
