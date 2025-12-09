---
title: "Blog 5"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.6. </b> "
---

# Cách kết nối robot của bạn với AWS Cloud và thúc đẩy đổi mới dựa trên dữ liệu

Khi nhắc đến robot, bạn có chỉ nghĩ đến phần phần cứng? Hay bạn nghĩ đến việc tạo ra các giải pháp robotics có thể đạt mức độ tự động hóa cao hơn và tối ưu hóa các quy trình phức tạp?

Thực tế, công nghệ robotics không chỉ là phần cứng – những thiết bị vật lý này phụ thuộc vào các hệ thống huấn luyện và trí tuệ nhân tạo để vận hành hiệu quả. Ngày nay, robot có thể thực hiện tự động hóa vượt xa các mô hình lập trình cứng nhắc, học hỏi từ chính trải nghiệm của mình và đưa ra quyết định dựa trên dữ liệu mà chúng thu thập từ môi trường xung quanh.

Cuối cùng, một robot chỉ tốt khi khả năng thu thập, lưu trữ và xử lý dữ liệu của nó tốt.

Dữ liệu chính là động lực cho đổi mới trong robotics – giúp cải thiện cả phần cứng lẫn phần mềm điều khiển.

Không có dữ liệu, bạn không thể:
- Xây dựng dashboard giám sát robot fleet để khắc phục sự cố.
- Huấn luyện mô hình Machine Learning (ML) có thể tái sử dụng.
- Chạy mô phỏng (simulation) để kiểm thử và xác thực tính năng mới với chi phí thấp hơn.

Chiến lược dữ liệu của bạn sẽ quyết định liệu bạn có thể xây dựng giải pháp bền vững từ đầu, liên tục đổi mới cho khách hàng và đạt hiệu quả vận hành tối ưu hay không.

Trong bài viết này, chúng tôi sẽ chia sẻ những gì đã học được khi làm việc với hàng trăm nhà phát triển robot để giúp họ xây dựng nền tảng kết nối vững chắc, cho phép tối ưu hóa và đổi mới dựa trên dữ liệu – ngay cả khi robot hoạt động ở những khu vực xa xôi, khắc nghiệt, nơi kết nối mạng có thể bị gián đoạn hoặc không ổn định.

Bạn cũng sẽ tìm hiểu cách kết nối robot ROS2 của mình với AWS IoT Core, cùng những cơ hội mở ra cho các use case và ứng dụng mới khi dữ liệu của robot được đưa lên AWS Cloud.


---

## Vì sao kết nối cloud-to-edge lại quan trọng với robotics

Việc triển khai robot trong môi trường sản xuất rất phức tạp, đòi hỏi xây dựng nhiều lớp trí tuệ như navigation, behavior, AI và ML.

Nhiều use case yêu cầu robot phải trao đổi và hợp tác với các robot khác, các thiết bị IoT, con người, và nhiều hệ thống điều khiển khác.

Ví dụ, trong một use case di chuyển vật liệu trong nhà máy, một đội robot công nghiệp cần phối hợp nhịp nhàng với hệ thống quản lý công việc **(work management system)** và nhiều **Programmable Logic Controllers (PLCs)** để đảm bảo hàng hóa di chuyển trơn tru, đạt throughput tối ưu.

Khái niệm **Internet of Robotic Things (IoRT)** và **Industrial IoT (IIoT)** đã phát triển nhanh chóng, tận dụng dữ liệu từ các cảm biến, bộ truyền động **(actuators)** và các thiết bị tự động **(autonomous things)** trong môi trường tiêu dùng, kinh doanh và công nghiệp.

IoRT kết hợp các hệ thống robot tự động với kết nối **IoT/IIoT**, **điện toán đám mây (cloud)** và **AI/ML**, giúp đẩy nhanh quá trình phát triển các giải pháp thông minh dựa trên dữ liệu.

AWS tin rằng dữ liệu do robot tạo ra là chìa khóa để nâng cao độ tin cậy và khả năng của hệ thống robot.

Kết nối robot chính là nền tảng cho phép bạn thu thập, xử lý dữ liệu, cải thiện chức năng, tăng khả năng quan sát hiệu suất và hoạt động.
Tuy nhiên, không phải môi trường nào cũng có kết nối ổn định và băng thông cao đến cloud.

Vì vậy, bộ dịch vụ trong **AWS IoT** và **Hybrid Cloud with AWS portfolio** được thiết kế để hỗ trợ khách hàng trong mọi tình huống – từ robot kết nối hoàn toàn với cloud, đến **robot hoạt động độc lập (disconnected)**.

Giờ hãy cùng tìm hiểu cách kết nối **robot ROS2** với **AWS IoT Core** một cách an toàn, và bắt đầu thu thập dữ liệu telemetry để nâng cao ứng dụng robotics của bạn.


---

## Kết nối robot ROS2 với AWS Cloud và bắt đầu thu thập dữ liệu telemetry
Trong phần này, bạn sẽ học cách gửi telemetry data từ robot sử dụng **ROS2** đến **AWS IoT Core** thông qua **MQTT protocol**.

Phần code được thử nghiệm trên **Ubuntu 22.04** với **ROS2 Humble**.
Bạn cần cấu hình certificates để kết nối thiết bị với **AWS IoT Core**, sử dụng **AWS CLI** cùng quyền truy cập **AWS Console** phù hợp.

#### 1. Cài đặt AWS CLI
```yaml
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install
```
#### 2. Thiết lập thông tin đăng nhập (credentials)
Bài viết giả định bạn là **admin user**, nhưng AWS khuyến nghị nên giới hạn quyền chỉ cho các thao tác cần thiết trong AWS IoT.

```yaml
export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE

export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

export AWS_DEFAULT_REGION=us-west-2
```
#### 3. Cài đặt AWS IoT Device SDK cho Python
```yaml
python3 -m pip install awsiotsdk
```
#### 4. Clone mã nguồn ví dụ
```yaml
cd ~

git clone https://github.com/aws-samples/aws-iot-robot-connectivity-samples-ros2.git
```
#### 5. Build mã nguồn
```yaml
cd ~/aws-iot-robot-connectivity-samples-ros2/workspace

colcon build

source ~/aws-iot-robot-connectivity-samples-ros2/workspace/install/setup.bash
```

Trước khi chạy, bạn cần tạo certificates để giao tiếp với **AWS IoT Core**.
Certificates giúp xác thực (authentication), phân quyền (authorization), và mã hóa dữ liệu truyền (encryption in transit).

Mỗi robot cần có credential riêng để tương tác với AWS IoT.
Mọi dữ liệu gửi đi và nhận về đều được bảo vệ bởi **TLS (Transport Layer Security)**.
![Send and receive in TLS](/images/CertificateAuth.png)

Bạn chịu trách nhiệm quản lý **device credentials, X.509 certificates**, cùng policies và permissions trong AWS IoT.

X.509 certificates giúp xác thực thiết bị với AWS IoT, đồng thời cung cấp bảo mật mạnh hơn so với username/password hoặc bearer tokens, vì **private key không bao giờ rời khỏi thiết bị**.

Trong môi trường sản xuất quy mô lớn, có thể dùng **AWS IoT Device Defender** để **kiểm toán (audit), phát hiện rủi ro bảo mật (security posture)**, và tự động thực hiện các biện pháp khắc phục như cập nhật certificate hoặc cách ly thiết bị.

---

## Thiết lập chứng chỉ (certificates) và cấu hình robot

Bạn sẽ cần thiết lập một số **biến môi trường (environment variables)** để chạy các lệnh API của AWS IoT.

Các biến này quy định nơi lưu chứng chỉ, tên của robot, và vị trí các file cấu hình mẫu.

```yaml
export CERT_FOLDER_LOCATION=~/aws-iot-robot-connectivity-samples-ros2/iot_certs_and_config/

export THING_NAME=my_ros2_robot_thing

export IOT_CONFIG_TEMPLATE=~/aws-iot-robot-connectivity-samples-ros2/templates/iot_config_template.json

export IOT_POLICY_TEMPLATE=~/aws-iot-robot-connectivity-samples-ros2/templates/iot_policy_template.json

export IOT_POLICY_NAME=ros2_robot_iot_policy
```
Lệnh trên sẽ tải các chứng chỉ (certificates) vào thư mục ```$CERT_FOLDER_LOCATION``` trên robot.

Tiếp theo, tải root certificate để kết nối AWS IoT Thing của bạn với AWS IoT Core, và liên kết (attach) chứng chỉ đã tạo vào Thing.

AWS IoT cung cấp client certificates được ký bởi **Amazon Root CA (Certificate Authority)**, giúp xác minh tính xác thực của kết nối.

```yaml
ROOT_CERT_FILE=$CERT_FOLDER_LOCATION"rootCA".crt

curl https://www.amazontrust.com/repository/AmazonRootCA1.pem > $ROOT_CERT_FILE

export CERT_ID=${CERT_ARN#*cert/}

aws iot attach-thing-principal --principal $CERT_ARN --thing-name $THING_NAME
```

Hiện tại, bạn đã có **chứng thực (authentication)** để kết nối với **AWS IoT Core**, nhưng vẫn cần một policy để định nghĩa permissions và giới hạn truy cập cho thiết bị (AWS IoT Thing) này.

Repository mẫu đã bao gồm một sample policy để thử nghiệm – bạn có thể chỉnh sửa tùy theo nhu cầu thực tế.

---

## Cấu hình file ```iot_config.json```

Tạo bản sao từ file mẫu ```iot_config_template.json```, sau đó điền endpoint và đường dẫn chứng chỉ tương ứng của robot:
```yaml
export ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

export ENDPOINT_ADDRESS=$(aws iot describe-endpoint --endpoint-type iot:Data-ATS --query endpointAddress --output text)

export PORT=8883

export IOT_CONFIG_FILE=~/aws-iot-robot-connectivity-samples-ros2/iot_certs_and_config/iot_config.json

cat $IOT_CONFIG_TEMPLATE >> $IOT_CONFIG_FILE

export PRIV_KEY_LOCATION=$CERT_FOLDER_LOCATION$THING_NAME.private.key

export CERT_FILE=$CERT_FOLDER_LOCATION$THING_NAME.cert.pem

sed -i -e "s/ENDPOINT/$ENDPOINT_ADDRESS/g" $IOT_CONFIG_FILE

sed -i -e "s/ROOTCA/$(echo $ROOT_CERT_FILE | sed 's_/_\\/_g')/g" $IOT_CONFIG_FILE

sed -i -e "s/PRIVATEKEY/$(echo $PRIV_KEY_LOCATION | sed 's_/_\\/_g')/g" $IOT_CONFIG_FILE

sed -i -e "s/CERTPATH/$(echo $CERT_FILE | sed 's_/_\\/_g')/g" $IOT_CONFIG_FILE

sed -i -e "s/CLIENT/$THING_NAME/g" $IOT_CONFIG_FILE

sed -i -e "s/PORT/$PORT/g" $IOT_CONFIG_FILE

cat $IOT_CONFIG_FILE

```
---

## Tạo IoT Policy

File policy mẫu định nghĩa quyền hạn cho AWS IoT Thing.

Mặc định, policy này chỉ cho phép publish và nhận dữ liệu từ topic ```ros2_mock_telemetry_topic.```
Bạn có thể mở rộng phạm vi khi cần thêm use case mới.

```yaml
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:Receive",
        "iot:RetainPublish"
      ],
      "Resource": [
        "arn:aws:iot:us-west-2:ACCOUNT_ID:topic/ros2_mock_telemetry_topic"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Subscribe"
      ],
      "Resource": [
        "arn:aws:iot:us-west-2:ACCOUNT_ID:topicfilter/ros2_mock_telemetry_topic"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Connect"
      ],
      "Resource": [
        "arn:aws:iot:us-west-2:ACCOUNT_ID:client/CLIENT"
      ]
    }
  ]
}

```

Tạo policy từ template và áp dụng cho Thing của bạn:

```yaml
export IOT_POLICY_FILE=~/aws-iot-robot-connectivity-samples-ros2/iot_certs_and_config/iot_policy.json

cat $IOT_POLICY_TEMPLATE >> $IOT_POLICY_FILE

sed -i -e "s/ACCOUNT_ID/$ACCOUNT_ID/g" $IOT_POLICY_FILE

sed -i -e "s/CLIENT/$THING_NAME/g" $IOT_POLICY_FILE

cat $IOT_POLICY_FILE

aws iot create-policy --policy-name $IOT_POLICY_NAME --policy-document file://$IOT_POLICY_FILE

aws iot attach-policy --policy-name $IOT_POLICY_NAME --target $CERT_ARN

```
Bạn sẽ thấy phản hồi như thế này:
![Wow, a matrix lines of code](/images/MatrixOfCode.png)

Khi hoàn tất, bạn đã sẵn sàng gửi dữ liệu đến AWS IoT Core.
Bây giờ, bạn có thể xóa AWS CLI credentials khỏi robot, vì nó đã được xác thực thông qua IoT Certificates.

---

## Chạy ROS2 Node và kiểm tra kết nối

Khởi chạy ROS2 node để gửi dữ liệu telemetry mẫu (mock telemetry):

```yaml
source ~/aws-iot-robot-connectivity-samples-ros2/workspace/install/setup.bash
ros2 run telemetry_mqtt mock_telemetry_pub
```

Node này sẽ publish dữ liệu giả lên topic ```mock_telemetry```.
 Bạn có thể kiểm tra dữ liệu này bằng lệnh:
```yaml
ros2 topic echo mock_telemetry
```

Sau đó, chạy node subscribe và publish dữ liệu này lên AWS IoT Core thông qua MQTT topic:
```yaml
export IOT_CONFIG_FILE=~/aws-iot-robot-connectivity-samples-ros2/iot_certs_and_config/iot_config.json

source ~/aws-iot-robot-connectivity-samples-ros2/workspace/install/setup.bash

ros2 run telemetry_mqtt mqtt_telemetry_pub --ros-args --param path_for_config:=$IOT_CONFIG_FILE

```

Để xem dữ liệu đã được publish, truy cập **AWS Console → IoT Core → MQTT test client**,
sau đó subscribe vào topic ```ros2_mock_telemetry_topic```.
Bạn sẽ thấy các gói dữ liệu telemetry xuất hiện trực tiếp trên giao diện.
![First, find for it](/images/SearchIoTCore.png)

![Then, click on MQTT](/images/MQTT.png)

![You will see the MPTT UI here](/images/MQTT.png)

Như vậy, robot ROS2 của bạn đã có thể gửi dữ liệu lên AWS IoT Core thành công.
Từ đây, bạn có thể mở rộng để chuyển đổi bất kỳ ROS2 topic data nào sang JSON-formatted messages và gửi qua MQTT.

---

## Lợi ích của kết nối Internet of Robotics Things (IoRT)

Khi càng nhiều thiết bị robot được kết nối trong **các đội tự động (autonomous fleets)**,
kiến trúc kết nối và nền tảng phân tích dữ liệu trở nên ngày càng quan trọng.
Những robot này tạo ra lượng data khổng lồ, giúp bạn:

- Giám sát (monitor) đội robot ở quy mô lớn.
- Quản lý và cập nhật phần mềm robot từ xa.
- Cải thiện hiệu suất thông qua dữ liệu thực tế.
- Thúc đẩy **liên tục đổi mới (continuous improvement)**.


Khi dữ liệu đã có trong AWS IoT Core, bạn có thể tận dụng kết nối IoRT để triển khai nhiều use case nâng cao hiệu quả hệ thống robotics của mình.

#### 1. Device Management

Các công cụ **AWS IoT Device Management** giúp bạn dễ dàng đăng ký, tổ chức, giám sát và quản lý các thiết bị được kết nối ở quy mô lớn.
Bạn cũng có thể truy cập từ xa vào robot để debug các vấn đề tại site thông qua **SSH tunneling** an toàn trên trình duyệt, sử dụng **AWS Device Client**.

#### 2. Operational Excellence và Continuous Improvement

Bạn có thể khai thác các insight vận hành bằng cách đưa dữ liệu vào AWS Analytics tools.
Ví dụ, bạn có thể **truyền dữ liệu (pipe data)** từ **IoT Core** đến **Amazon OpenSearch Service** bằng **IoT Rule**,
rồi hiển thị trên **Amazon Managed Grafana dashboard** để theo dõi các metrics và hiệu suất.

#### 3. Fleet Monitoring và Teleoperation

Kết hợp các dịch vụ **AWS IoT** như **MQTT, Device Shadow**, cùng **Amazon Kinesis Video Streams**,
bạn có thể xây dựng các giải pháp **command-and-control** và **teleoperation** mở rộng, an toàn, cho toàn bộ đội robot của mình.

#### 4. Xây dựng robot thông minh nhanh hơn

**AWS IoT Greengrass** là **IoT edge runtime mã nguồn mở** và **cloud service** hỗ trợ triển khai và quản lý giải pháp robotics ở quy mô lớn.
Greengrass cung cấp nhiều component sẵn có như:
Quản lý bảo mật thiết bị và credentials

- Truy cập từ xa qua SSH
- Triển khai và chạy mô hình ML
- Ghi log, truyền dữ liệu đệm (buffered streaming)
- Triển khai phần mềm đồng loạt cho robot fleet


Bạn có thể dùng Greengrass để **provision, deploy và quản lý toàn bộ vòng đời phần mềm cho đội robot**, bao gồm cả các robot dựa trên ROS.

## Các bước tiếp theo

Với AWS, bạn có quyền truy cập vào bộ công cụ và tài nguyên toàn cầu để đơn giản hóa việc phát triển và vận hành robot của mình.

Hãy bắt đầu ngay hôm nay bằng cách kết nối robot với **AWS Cloud** thông qua **sample application**,
và bắt đầu thu thập telemetry data thực tế.

Trong khi bạn ở trên **GitHub**, đừng quên **“favorite” repository** để nhận thông báo khi có bản cập nhật mới!
