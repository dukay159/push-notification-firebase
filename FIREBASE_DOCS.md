# TÌM HIỂU PUSH NOTIFICATION TRONG FIREBASE
## I. FIREBASE
1. Khái niệm
    * Firebase là một dịch vụ API để lưu trữ và đồng bộ dữ liệu real-time (thời gian thực). Điều này có nghĩa là bạn không cần phải lo lắng về backend server, cơ sở dữ liệu, hay các thành phần real-time (socket.io).
    * Firebase hoạt động trên nền tảng đám mây được cung cấp bởi Google nhằm giúp các lập trình phát triển nhanh các ứng dụng bằng cách đơn giản hóa các thao tác với cơ sở dữ liệu.
2. Các chức năng hoạt động chính
    * *Fibase Authentication:* là xây dựng những bước xác dụng người dùng thông qua Email, Facebook, Twitter, GitHub hay Google. Ngoài ra, hoạt động Firebase Authentication cũng hỗ trợ xác thực nặc danh cho những ứng dụng. Hoạt động xác thực của Firebase có thể giúp cho thông tin cá nhân của những người sử dụng được an toàn hơn.
    * *Firebase Hosting:* là một hoạt động được phân phối thông qua tiêu chuẩn công nghệ bảo mật SSl từ hệ thống mạng CDN - một mạng lưới máy chủ giúp lưu giữ lại các bản sao của các nội dung tĩnh, Những nội dung tĩnh này nằm ở bên trong website và trực tiếp phân phối đến các máy chủ PoP khác.
    * *Firebase Realtime Database:* có dạng một JSON đã được đồng bộ thời gian đến với tất cả các kết nối client. Dữ liệu ở trong database sẽ tự động cập nhật một cách liên tục khi phát triển ứng dụng. Sau khi đã được cập nhật thì những dữ liệu này sẽ được truyền tải thông qua các kết nối SSl có 2048 bit. 
3. Tạo 1 dự án trong Firebase

    B1: Truy cập và đăng nhập vào trang web: https://console.firebase.google.com

    B2: Click vào *__Create a project__* và hoàn thành 3 step

    \* **Lưu ý:** Ở step 3 phần chọn account có thể lựa chọn \*Default Account for Firebase\* trong trường hợp chưa có account
## II. ADD FIREBASE TO ANDROID STUDIO

    B1: Tạo 1 dự án hoặc dùng 1 dự án có sẵn trong Android Studio

    B2: Truy cập vào trang web [https://console.firebase.google.com] và chọn biểu tượng android để khởi tạo project firebase cho android

    B3: Thực hiện các step:

    *Register app:
        - Phần Android package name : Là tên package của dự án trong Android Studio (có thể lấy trong file "manifests")

        - Phần App nickname: Đặt tên cho project trong Firebase 

        - Phần mã SHA-1: Lấy mã bằng cách mở Android Studio -> Click Gradle (ở góc trên cùng bên phải màn hình) -> *Tên dự án* -> Tasks -> android -> signingReport => mã SHA-1 xuất hiện ở cửa sổ run

        - Click *Register app*

    *Download config file:
        - Click button download file "google-service.json"
        
        - Copy file "google-service.json" vừa tải về vào folder "app" trong project Android Studio 

    *Add Firebase SDK:
        - Copy code vào file "build.gradle\<project>/build.gradle" và file "build.gradle\<app-module>/build.gradle"

        - Click *Sync now*

    *Next steps:

        Click *Continue to console*
## III. PUSH NOTIFICATION TRONG ANDROID
1. Push Notification kiểu *Notification Messages*
    
    \* Tham khảo các bước trong link: https://firebase.google.com/docs/cloud-messaging/android/client . Các bước có ghi (Optional) thì có thể làm theo hoặc không

    \* Source code sample: https://github.com/dukay159/push-notification-firebase/tree/type-notification-messages

    \* Các bước thực hiện:

    B1: Để sử dụng dịch vụ Firebase Cloud Messaging (FCM) cần import thư viện *__implementation 'com.google.firebase:firebase-messaging'__*

    B2: Tạo channel ID cho Push Notification: Tham khảo file *__My Application__* trong code

    B3: Xử lý nhận Notification từ FireBase:
     - Tham khảo file *__MyFirebaseMessagingService__*
     - Đăng kí service trong file *__manifests__*
    
    B4: Gửi Push Notification trực tiếp từ Firebase:
     - Truy cập vào dự án đã tạo trên Firebase ở link: https://console.firebase.google.com
     - Click vào *__Messaging__* phía góc trái màn hình
     - Click *__Create your first campaign__* và điền các thông tin khởi tạo notification
2. Push Notification kiểu *Data Messages*

    \* Source code sample: https://github.com/dukay159/push-notification-firebase/tree/type-data-messages
    
    \* Về mặt hoạt động thì tương tự như kiểu Notification Messages, tuy nhiên thay vì gửi trực tiếp từ Firebase thì sẽ gửi từ Server thông qua Firebase để gửi notification đến các device client

    \* Các bước hoạt động: 

        B1: Server ứng dụng lựa chọn device cần thông báo, tiến hành lấy token firebase + data gửi đến cho phía Firebase server.

        B2: Firebase server sẽ xử lý và gửi tin nhắn đến đúng device cần nhận thông báo, Firebase server đóng vai trò như trung gian cho việc gửi tin nhắn.
        
        B3: Ứng dụng của người dùng sẽ nhận được tin nhắn từ phía Firebase gửi dưới dạng “data payload”, lắng nghe các sự kiện từ hàm “onMessageReceived”, phân tích và tạo notification đến người dùng.
    \* Các bước thực hiện:
        
        B1: Get token: Tham khảo  file "MainActivity". Lưu ý: token chỉ lấy được khi chạy app lần đầu

        B2: Sử dụng Postman để call Api gửi Push Notification:
         - Method: POST
         - Url API : https://fcm.googleapis.com/fcm/send
         - Header:
            Authorization : key="SERVER API KEY"
            * Để lấy được "SERVER API KEY" mở "Project settings" trong dự án: https://console.firebase.google.com ->  Cloud Messaging -> Manage API in Google Cloud Console(dấu ... đứng bên phải phần Cloud Messaging API) -> Enable 
            
            Content-Type:application/json
        - Body:
          { 
            "data": {
                "key_1": "value_1",
                "key_2": "value_2"
                *key tương ứng lấy trong code, tham khảo hàm "onMessageReceived" trong file "MyFirebaseMessagingService" 

                },
               "to" : "token_firebase_of_device" (token đã get)
          }
## IV. TÀI LIỆU THAM KHẢO
 - Account test Firebase: 
    
    + gmail: joyfoodtest@gmail.com
    + pass: Zensho@123
 - Code sample: https://github.com/dukay159/push-notification-firebase.git

 - FCM: https://firebase.google.com/docs/cloud-messaging
