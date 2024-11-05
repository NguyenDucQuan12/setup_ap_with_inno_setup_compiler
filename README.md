# Hướng dẫn cách đóng gói tệp exe thành một file cài đặt duy nhất

### Để đóng gói thành một file cài đặt duy nhất ta sử dụng Inno setup compiler

Tạo một folder chứa các tệp cần thiết như: file exe, các thư mục hình ảnh, log, ...  

![Cấu trúc thư mục](image/folder_app.png)

Sau đó mở `inno setup compiler`:  

![Mở inno setup compiler](image/inno_setup_compiler.png)

Sau đó chọn `Cretae a new script file using the Script Wizad` và nhấn `OK`. Và ấn `Next` khi hộp thoại mở lên.  

![Không chọn tạo file mới](image/new_empty_script.png)

Tiếp theo điền các thông tin quan trọng về phần mềm như ảnh dưới:  

![Điền thông tin về phần mềm](image/information_app.png)

`Application name` sẽ là tên phần mềm mà bạn muốn hiển thị  
`Application version` là phiên bản phần mềm  
`Application publisher` là tên người tạo phần mềm  
`Application website` là trang web phần mềm nếu có  

Tiếp theo sẽ chọn nơi đặt thư mục phần mềm  

![Nơi đặt thư mục app](image/folder_app_install.png)

`Application destination base folder` là nơi đặt thư mục app, `Program Files Folder` sẽ là đặt ở ổ `C:/Program File (x86)`  
`Application folder name` là tên thư mục sẽ đặt ở mục `Program Files Folder`: Nên đặt là `Smart Parking Program`  

Sau đó cần thêm các tệp exe và thư mục cần thiết cho phần mềm  

![Thêm tệp exe và thư mục cần thiết](image/add_file_and_folder.png)

Đầu tiên cần phải thêm tệp `exe là chương trình chính` để file cài đặt thêm vào, sau đó thêm các thư mục cần thiết như `image`, `data`, ... Chỉ cần thêm các thư muc chính, các file trong đó nó sẽ tự động copy vào  

![Bỏ tich mục tạo file myp](image/create_myp_file.png)

Cần bỏ chọn vào mục `Associate a file type to the main executable`  

![Tạo short cut](image/short_cut_app.png)

Ấn `next` và sau đó cung cấp các file license  

![Các tệp license](image/license.png)

Sau đó chọn chế độ cài đặt yêu cầu quyền admin, như vậy nó sẽ cài đặt thằng vào thư mục `C:/Program File (x86)` Nếu không nó sẽ không có quyền cài đặt ở thư mục ổ C.  

![Cài đặt chế độ admin](image/admin_install.png)

## Một số lưu ý với chế độ cài đặt  
### Chọn chế độ cài đặt Admin  
Nghĩa là chương trình sẽ được cài vào thư mục `C:\\Program File (x86)\\ My app`  
Như vậy nếu chương trình của bạn có các lệnh `thêm, sửa, xóa` các tệp tin trong chương trình, nghĩa là thay đổi các tệp trong thư mục ` ổ C` thì bắt buộc lúc khởi chạy phần mềm thì bạn phải chạy nó với quyền admin, hay `run as administrator`  

### Chọn chế độ cài đặt non Admin  
Thì chương trình sẽ được cài vào một thư mục mà `user` nào cũng có quyền truy cập ` thêm sửa xóa ` thì khi khởi chạy nó sẽ không cần chạy với quyền admin, mà chỉ cần nhấn đúp vào  

### Chọn chế độ hỏi chế độ cài đặt khi cài  

Chọn chế độ này để người dùng tự chọn chế độ cài đặt  

![alt text](image/install_mode.png)

## 

Chọn ngôn ngữ mặc định là `English` và ấn next:  

![alt text](image/Language.png)

Chọn nơi để xuất ra file cài đặt, đặt `tên cho file setup` và `icon` của file setup.  

![alt text](image/save_setup.png)

Ấn `next` và `finish`  

![alt text](image/use_define.png)

Nó sẽ ra 1 file và nhớ chọn `No` để chỉnh sửa file này như sau.  

![alt text](image/no_compiler_script.png)

Chỉnh sửa file có một vài điểm sau:  
Chương trình của tôi có 2 tệp exe, trong đó có 1 tệp exe mang tên `Updater.exe` đây là chương trình dùng để cập nhật phần mềm chính, nó cần được thêm vào thư mục nên tôi đã thêm 1 dòng bên dưới dòng chứa ứng dụng chính của tôi là `#MyAppExeName` và `Updater.exe`  

```
[Files]
Source: "D:\Project\Smartparking_app\{#MyAppExeName}"; DestDir: "{app}"; Flags: ignoreversion
Source: "D:\Project\Smartparking_app\Updater.exe"; DestDir: "{app}"; Flags: ignoreversion
```

Các thư mục trong phần mềm của tôi cần phải chỉnh sửa để nó tạo ra thư mục đúng:  
Trong code của tôi truy cập các tệp theo đường dẫn `assets/image` và `data` nên ta cần thêm đúng chính xác thứ tự ta muốn tạo folder: `DestDir: "{app}/assets/image"`, `DestDir: "{app}/data"` Nếu không mặc định nó sẽ không tạo thư mục mà sẽ để hết vào cũng 1 chỗ `root`.  
```
[Files]
Source: "D:\Project\Smartparking_app\{#MyAppExeName}"; DestDir: "{app}"; Flags: ignoreversion
Source: "D:\Project\Smartparking_app\Updater.exe"; DestDir: "{app}"; Flags: ignoreversion
Source: "D:\Project\Smartparking_app\assets\image\*"; DestDir: "{app}/assets/image"; Flags: ignoreversion recursesubdirs createallsubdirs
Source: "D:\Project\Smartparking_app\data\*"; DestDir: "{app}/data"; Flags: ignoreversion recursesubdirs createallsubdirs
Source: "D:\Project\Smartparking_app\src\*"; DestDir: "{app}/src"; Flags: ignoreversion recursesubdirs createallsubdirs
```

Cuối cùng ấn `Compiler`  

![Compiler](image/compiler.png)

Đợi một lúc để nó hoàn tất qusa trình đóng gói, ta sẽ được tệp cài đựt như sau:  

![alt text](image/setup_app.png)

Và ta chỉ cần chạy file đó là quá trình cài đặt diễn ra như bình thường.

Ví dụ khi khởi động:

Chạy file cài đặt:  

![alt text](image/install_mode.png)

Chọn chế độ cài đặt tương ứng để tiếp tục, mình chọn `Install for all user (recommended)`  

Sau đó đến mục điều khoản:  

![alt text](image/license_before_install.png)  

Nhấn `accept` và tiếp tục:  

![alt text](image/my_license.png)

Sau đó chọn thư mục để cài đặt phần mềm của mình.

![alt text](image/location_app.png)

Ấn `next` và lựa chọn tạo `shortcut` cho phần mềm.  

![alt text](image/create_shortcut.png)

Để hoàn thành thì nhất `install`  

![alt text](image/install_exe.png)

Sau đó đọc thêm file `license` cuối và khởi động chương trình khi đã cài đặt xong.  

![alt text](image/last_license.png)

![alt text](image/run_program.png)

> Nếu chương trình có thêm sửa xóa thì phải khởi động nó với quyền admin `run as administrator` nha  
> Một cách đơn giản hơn là nếu có các tệp cần phải chỉnh sửa thì hãy tạo nó nằm ở một thư mục khác, không nên để thư mục đó ở ổ C