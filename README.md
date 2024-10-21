# setup_ap_with_inno_setup_compiler
Đóng gói ứng dụng thành một tệp cài đặt

Tạo một folder chứa các tệp cần thiết như: file exe, các thư mục hình ảnh, log, ...  

![Cấu trúc thư mục](image/folder_app.png)

Sau đó mở `inno setup compiler`:  

![Mở inno setup compiler](image/inno_setup_compiler.png)

Sau đó chọn `Cretae a new script file using the Script Wizad` và nhấn `OK`. Và ấn `Next` khi hộp thoại mở lên.  

![Không chọn tạo file mới](image/new_empty_script.png)

Tiếp thoe điền các thông tin quan trọng về phần mềm như ảnh dưới:  

![Điền thông tin về phần mềm](image/information_app.png)

`Application name` sẽ là tên phần mềm mà bạn muốn hiển thị  
`Application version` là phiên bản phần mềm  
`Application publisher` là tên người tạo phần mềm  
`Application website` là trang web phần mềm nếu có  

Tiếp theo sẽ chọn nơi đặt thư mục phần mềm  

![Nơi đặt thư mục app](image/folder_app_install.png)

`Application destination base folder` là nơi đặt thư mục app, `Program Files Folder` sẽ là đặt ở ổ `C:/Programfile`  
`Application folder name` là tên thư mục sẽ đặt ở mục `Program Files Folder`: Nên đặt là `Smart Parking Program`  

Sau đó cần thêm các tệp exe và thư mục cần thiết cho phần mềm  

![Thêm tệp exe và thư mục cần thiết](image/add_file_and_folder.png)

Đầu tiên cần phải thêm tệp `exe là chương trình chính` để file cài đặt thêm vào, sau đó thêm các thư mục cần thiết như `image`, `data`, ... Chỉ cần thêm các thư muc chính, các file trong đó nó sẽ tự động copy vào  

![Bỏ tich mục tạo file myp](image/create_myp_file.png)

Cần bỏ chọn vào mục `Associate a file type to the main executable`  

![Tạo short cut](image/short_cut_app.png)

Ấn `next` và sau đó cung cấp các file license  

![Các tệp license](image/license.png)

Sau đó chọn chế độ cài đặt không yêu cầu quyền admin, nếu không mỗi lần chạy nó sẽ yêu cầu admin, hơi rắc rối.  

![Cài đặt chế độ không admin](image/Non-admin_install.png)

Chọn ngôn ngữ mặc định là `English` và ấn next:  

![alt text](image/Language.png)

Chọn nơi để xuất ra file cài đặt, đặt tên cho file setup và icon phù hợp.  

![alt text](image/save_setup.png)

Ấn `next` và `finish`  

![alt text](image/use_define.png)

Nó sẽ ra 1 file và nhớ chọn `No` để chình sửa file này như sau.  

![alt text](image/no_compiler_script.png)

Chỉnh sửa file có một vài điểm sau:  
Chương trình của tôi có 2 tệp exe, trong đó có 1 tệp exe mang tên `Updater.exe` đây là chương trình dùng để cập nhật phần mềm chính, nó cần được thêm vào thư mục nên tôi đã thêm 1 dòng dưới

```
[Files]
Source: "D:\Project\Smartparking_app\{#MyAppExeName}"; DestDir: "{app}"; Flags: ignoreversion
Source: "D:\Project\Smartparking_app\Updater.exe"; DestDir: "{app}"; Flags: ignoreversion
```

Các thư mục trong phần mềm của tôi cần phải chỉnh sửa để nó tạo ra thư mục đúng:  
```
[Files]
Source: "D:\Project\Smartparking_app\{#MyAppExeName}"; DestDir: "{app}"; Flags: ignoreversion
Source: "D:\Project\Smartparking_app\Updater.exe"; DestDir: "{app}"; Flags: ignoreversion
Source: "D:\Project\Smartparking_app\assets\image\*"; DestDir: "{app}/assets/image"; Flags: ignoreversion recursesubdirs createallsubdirs
Source: "D:\Project\Smartparking_app\data\*"; DestDir: "{app}/data"; Flags: ignoreversion recursesubdirs createallsubdirs
Source: "D:\Project\Smartparking_app\src\*"; DestDir: "{app}/src"; Flags: ignoreversion recursesubdirs createallsubdirs
```
Để nó tạo ra các thư mục `assets/image`, `data`, `src`, ... Nếu không nó sẽ không tự tạo thư mục  
Cuối cùng ấn `Compiler`  

![Compiler](image/compiler.png)